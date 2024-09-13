# Exporting SolidWorks to URDF

## Installing SolidWorks for Free on Windows
(Include installation steps, TBD)

## Steps to Create URDF from SolidWorks

### 1. Open the Assembly File
- Open the `.SLDASM` file from `hardware/follower/SolidWorks` or `hardware/leader/SolidWorks`.

   ![Open SLDASM file](./pictures/SW2URDF/open_SLDASM_file.png)

### 2. Group Parts
- Hide the parts you don't want grouped together by right-clicking and selecting "Hide."

   ![Other parts](./pictures/SW2URDF/other_parts.png)

   ![Grouped parts](./pictures/SW2URDF/grouped_parts.png)

- Save the remaining parts in a separate folder as `.SLDPRT` files. Name them systematically, e.g., `base_link`, `link_1`, etc., for easier tracking.

   ![All grouped parts](./pictures/SW2URDF/all_grouped_parts.png)

### 3. Modify Parts for Rotational Joints
- For parts that need rotational joints, open the corresponding part and draw a circular sketch. This circle should be tangent to the through-hole cylinders (holes for screws) to define the axis of rotation in the assembly.

   ![Draw circle sketch](./pictures/SW2URDF/circle_sketch.png)

- Save each modified part.

### 4. Create a New Assembly in SolidWorks
1. Open SolidWorks and create a new assembly.
2. Insert the saved parts and start assembling them using the **Mate** tool.
   - Start from `base_link` and link it to `link_1` using the **coincident mate** for surfaces.

      ![Coincident mate](./pictures/SW2URDF/coincidence_mate.png)

   - For rotational joints, use **concentric mate** for the cylindrical surfaces.

      ![Cocentric mate](./pictures/SW2URDF/cocentric_mate.png)

3. Continue assembling the remaining links using the parts you previously modified.

   ![End result of the assembly](./pictures/SW2URDF/end_result_of_the_assembly.png)

4. Align the robot arm using the **Mate** tool to make links parallel/perpendicular to each other.
   
   ![Parallel alignment with mate tool](./pictures/SW2URDF/parallel_mate.png)

   - After alignment, delete the parallel mate constraints to restore link movement while keeping the alignment.

      ![Remove parallel mates](./pictures/SW2URDF/remove_parallel_mates.png)

      ![After aligning each part](./pictures/SW2URDF/after_aligning_each_part_result.png)

5. Save the assembly regularly to avoid losing progress.

### 5. Define Coordinate Systems and Joint Axes
1. **Global Coordinate System:**
   - Use the **Reference Geometry** tool to define `x`, `y`, and `z` axes by selecting edges of the `base_link`.

      ![Create global axis](./pictures/SW2URDF/create_global_axis.png)

   - Define a **Reference Point** for the global origin, ideally at the center of the arm's base and at the lower point.

      ![Global Origin](./pictures/SW2URDF/Golobal_origin.png)

   - Create a **Coordinate System** using the point and axes defined above. Set the axis for this coordinate system using the global axes defined before. Ensure the global coordinate system follows the standard 3D frame (z-axis pointing upward, y-axis pointing away from you, and x-axis pointing right). Use the flip button in SolidWorks if necessary to adjust the orientations. Name this system `global_coordinates_system` for better tracking.

      ![Global coordinates system](./pictures/SW2URDF/Global_coordinates_system.png)

2. **Joint Axis and Link Frames:**
   - For each link, define the joint axis using the cylinder of rotation and name it (e.g., `joint_1`).

      ![Joint axis](./pictures/SW2URDF/joint_axis.png)

   - Define a **Reference Point** at the intersection of the joint axis and link surface.

      ![Joint reference point](./pictures/SW2URDF/joint_reference_point.png)

   - Create a **Coordinate System** for each link. Select the defined point as the origin and orient it using the appropriate axis. Ensure it aligns with the global coordinate system. Name it accordingly, e.g., `link_1_coordinates_system`, for better tracking.

      ![Link_1 coordinates system](./pictures/SW2URDF/link_1_coordinates_system.png)
   
   - Repeat for all joints/links.
  
All coordinates systems and joints axis:

![All coordinates systems and joint axis](./pictures/SW2URDF/all_coordinates_systems_and_joint_axis.png)

### 6. Export to URDF
1. Open the **URDF Exporter** from the **Tools** menu.
2. Set the base link (`base_link`) by selecting the global coordinate system manually.

   ![Fill the Global system add child link](./pictures/SW2URDF/fill_the_Global_system_add_child_link.png)

3. Increase the number of children by one (e.g., `link_1`).
4. Choose the empty link, give it a name, and name the joint as well (e.g., `joint_1`). Select the appropriate coordinate system and axis.
5. Set the joint type (e.g., revolute).

   ![Fill elements for the first link](./pictures/SW2URDF/fill_elements_for_the_first_link.png)

6. Repeat for all links and joints up to `link_6`.

   ![End of setting up urdf export](./pictures/SW2URDF/end_of_setting_up_urdf_export.png)

7. Click **Preview** and export.

### 7. Finalizing URDF Export
1. In the limits popup, set joint values:

   ![Fill the limits](./pictures/SW2URDF/fill_the_limits.png)

   - **Limits**: (-3.14, 3.14)
   - **Effort**: 100
   - **Velocity**: 0.5
   - Use specific values for the final link if needed (e.g., from Alex Koch files: (-2.45, 0.032), (0.5), (100)), then click **Next**.

2. Review the moments of inertia (default values are typically fine).

   ![Summary of the moment of inertia](./pictures/SW2URDF/summary_of_the_moment_of_inertia.png)

3. Click on Export URDF and meshes.

   ![End result after exporting](./pictures/SW2URDF/end_result_after_export.png)

   The image below shows how the exported files are organized. You can find the URDF file in the `urdf` folder and the `.stl` files in the `meshes` folder.

   ![Output file structure](./pictures/SW2URDF/output_file_structure.png)

### 8. Converting URDF to Mujoco XML (MJCF)
- Follow conversion steps from URDF to Mujoco XML format.