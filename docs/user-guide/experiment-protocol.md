# Experiment Protocol

This page describes the standard operating procedures for running Y-maze behavioral experiments, maintaining the odor delivery system, and preparing flies. These instructions were developed by the Turner Lab at Janelia Research Campus.

---

## Y-maze Assay Instructions

*Authors: Claire Manangan — Turner Lab, Janelia Research Campus (October 2024)*

### Schedule

Experiments are run in the **morning and afternoon on Wednesday, Thursday, and Friday**.

### Fly Preparation

1. Get the tray of flies for that day's experiment from **Incubator E** (21 °C, 9 am–1 am) near the Invertebrate Shared Resource (FlyCore).
2. The trays should be labeled **"Turner 16Y"**.
3. Look for the tray with a starvation date **2 days before the current date**:
    - Flies run on **Wednesday** are starved on **Monday**
    - Flies run on **Thursday** are starved on **Tuesday**
    - Flies run on **Friday** are starved on **Wednesday**
4. Take the flies to the Y-maze room (**2E.361.4**).

---

### Setting Up the Assay

1. Open the door to the rig and loosen the screws if needed.
2. Make sure the plates covering each Y-maze are positioned so the hole is above one of the arms.
3. Load **one fly into each Y-maze** using a mouth pipette, **excluding maze 0 and maze 3**.
4. Each tray should have approximately **40 control flies** and **40 experimental flies**.
5. If both control and experimental flies are being run, alternate which group goes first on each subsequent day.
6. Once a fly is in the maze, use the tip of the mouth pipette to slide the plate to the **closed position**.
7. Once all flies are loaded, tighten the screws and close the door.
8. Run the **`validator.bat`** script:
    - The script is in the project folder for the current experiment on `D:\AparnaAlt`.
    - It checks which experiments were run last and records the next experiments in `next_to_run.log`.
    - Press **Enter four times** to accept the default settings:
        - *"Enter number of replicates per experiment (leave empty for default = 1):"* → Enter
        - *"Enter minimum number of trials per experiment (leave empty for default = 180):"* → Enter
        - *"Enter minimum number of experiments per run (leave empty for default = 15):"* → Enter
        - *"Clear cache? (y/n) (default = n):"* → Enter
    - Once finished, press any key to close the Command Prompt window.
9. Execute the **`run_experiment.bat`** script located at `C:\16YArena\TurnerLab_Opto2AFC_16Y\run_experiment.bat`. This opens a Command Prompt window and then the **16-Y Experimenter** GUI.
10. In the **16-Y Experimenter** GUI:
    - Click **Browse** next to *Project Directory* and navigate to the project folder on `D:\AparnaAlt`.
    - For *Experiment Name*, calculate starvation duration from the start time written on the tray (typically **40–42 hours** in the morning, **44–46 hours** in the afternoon). Use the format:
        - `Xhr_starvation_EXP` for experimental flies
        - `Xhr_starvation_CONT` for control flies
    - Click **Add Timestamp** to append the current date and time.
    - For *Fly Genotype*, select the correct genotype from the dropdown. If not listed, choose `custom` and type the genotype name.
    - For *Starvation Start*, enter the starvation time calculated above and click **Update** to autofill the date/time. Edit to the closest half-hour if needed.
    - Click **Toggle All Arenas**, then uncheck **Fly 0** and **Fly 3**.
    - Click **Copy Genotypes**, **Copy Starvation Starts**, and **Copy Comments**.
    - Click **Auto-Populate Experiments from File** and select `next_to_run.log` from the current project directory.
    - Click **Start Experiment**.
11. When the *Create Rig Configuration File* pop-up appears, click **Yes** to open the **Rig Configurator** GUI:
    - Click **Browse** next to *Experiment Directory* and navigate to the data folder for the current project on `D:\AparnaAlt`. Select the folder with the correct date/timestamp.
    - Click **Load Configuration** and select `D:\AparnaAlt\config.yarena`.
    - Click **No** on the *Change Experiment Directory* pop-up.
    - Click **Save Configuration**, then **OK** on the *Configuration Saved* pop-up.
    - Click **Yes** on the *Save Configuration* pop-up to close the Rig Configurator.
12. Click **OK** on the *Rig Configuration File Present* window to begin running.
13. **Open the two air supply valves** and flip the switch on the black power unit (plugged into the surge protector) to **turn on the MFCs** (mass flow controllers).
14. Answer the questions in the Command Prompt window:
    - *"Have you turned on the MFCs and air supply? (y/n)"* → `y`
15. A background image will open in **Figure 1**. Check for any flies visible in the background:
    - No flies visible → *"Is this the background image? (y/n)"* → `y`
    - Flies visible → *"Is this the background image? (y/n)"* → `n`, then *"Background image capture failed. Retry? (y/n)"* → `y`
16. Once confirmed, a mask file will open in **Figure 1**. Verify no flies are visible in the mask, close the window, then:
    - *"Is this the mask? (y/n)"* → `y`
    - *"Start in debug mode? (y/n)"* → `y`
17. Experiments will now run and finish in approximately **2.5 hours**.

---

### Shutting Down and Resetting the Assay

1. **Close both air supply valves** and turn off the MFCs.
2. In the Command Prompt window: *"Did you turn off the air supply and MFCs? (y/n)"* → `y`
3. Close the Command Prompt window (this automatically closes the 16-Y Experimenter GUI).
4. Open the door to the rig and loosen all the screws.
5. Use a mouth pipette to **remove the flies** from each Y-maze:
    - Slide the plate covering the maze to the open position and suck up the fly.
6. Dispose of the flies in the **fly morgue**.
7. To run another experiment, repeat **Setting Up the Assay** steps 1–10.
8. After the **final experiment** of the day, repeat steps 1–4 above.

---

## Y-maze Odor Replacement Instructions

*Authors: Claire Manangan — Turner Lab, Janelia Research Campus (October 2024)*

Odor vials are replaced **each Tuesday**.

### Preparing New Odor Vials

1. Locate the two empty blue racks in the Y-maze room (**2E.361.4**):
    - One rack labeled **"MHO 1:1000"** (green tape)
    - One rack labeled **"HAL 1:1000"** (orange tape)
2. Take the racks to the bench at **2E.235.4** (Turner lab) and fill each with **16 new empty amber vials**.
    - Amber vials are stored in a box above the bench at 2E.235.3.
    - Notify Glenn if the supply is running low.
3. Get two **250 mL jars** from the glass supply closet and fill each with **150 mL of Paraffin oil**.
    - Label one jar **"MHO"** (green tape) and the other **"HAL"** (orange tape).
4. Take the jars to the **fume hood** in the shared weigh room (**2E.248**).
5. Put on gloves before handling odors. Odors are in the **yellow flammables cabinet** opposite the fume hood:
    - One bottle labeled **"MHO"**, one labeled **"HAL"**.
6. In the fume hood, add **15 µL (1:1000)** of each odor to its corresponding jar.
7. Add a small stir bar to each jar and spin for a few minutes at full power.
8. Get two **10 mL auto-pipette tips** from the LSR (2E.252). Fill each amber vial with **7.5 mL** of the corresponding odor solution.
9. Pour excess odor solution into the **liquid waste container** under the sink in the Turner lab.
10. Rinse the jars and place them in the glass cleaning trays.

### Replacing Odor Vials on the Rig

1. Take the newly filled racks to the Y-maze room.
2. Remove the old vials by twisting them **clockwise**.
    - Black rubber gaskets and thin plastic tubes may fall off — set gaskets aside and use the handheld mirror to replace any plastic tubes.
3. Attach the new vials by twisting them **counterclockwise**.
4. The rig has two "sets" of odor vials, each with three rows:
    - The **inner row** (closest to the middle) contains no odors — do not remove these.
    - The **middle row** of each set → **Odor 1 (MHO)**
    - The **outer row** (closest to the edge) → **Odor 2 (HAL)**
    - Viewed from the front: `Odor 2 – Odor 1 – Air :: Air – Odor 1 – Odor 2`
5. Check each vial position for a **black rubber gasket** using the handheld mirror. Place a gasket on top of any new vial before screwing it in if one is missing.
6. Periodically check the **water level** in the jars below the odor vials. Fill with DI water from the sink outside the Y-maze room (water should last a few weeks).
7. Return the old vials to the now-empty racks and take them to the bench at **2E.235.4**.
8. Put on gloves and pour the old vial contents into a beaker, then empty into the **liquid waste container** under the sink.
9. Rinse the beaker and place it in the glass cleaning tray.
10. Dispose of old amber vials in the **large sharps container** near the bench end.
    - If the container is full, get a new one from the LSR (2E.252).
11. Return the empty racks to the Y-maze room.
