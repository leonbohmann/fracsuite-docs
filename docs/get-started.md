---
title: Get Started
---

:::warning
Make sure that you follow the previous instructions before working through this page.
:::

## Creating a specimen

First, you need to create the folder structure for the specimen. You can either do that manually or you use the command

```
fracsuite specimen create [name]
```

where [name] is to be replaced with the name of the specimen. In my case, the specimen name was "4.100.B.01", then I would call `fracsuite specimen create 4.100.B.01`. After the command succeeded, a new folder is created in `database_folder`.

## Importing data

:::important
These scripts are currently only working for specimens with the name specification from [the intro](./intro.md#folder-structure).

**Acceleration data as well as the anisotropy scans still have to be copied into the specimen folder manually.**
:::

Normally, when conducting experiments, a small tool is used that already organizes all data correctly in the folder structure mentioned in the [introduction](intro.md#folder-structure). Then, you can just copy the whole folder structure using the File Explorer. In the case that you are creating specimen from scratch, the data has to be imported manually.


### SCALP Data
When SCALPing specimens, the scalp project file has to be saved and the "specimen" property must be set.

A prerequisite for the script is measurement at 3 points or more, each with at least 3 directions. If there are fewer than 3 directions at a measuring point, the missing measured value can be added manually while the script is running.

#### Collect projects
Collect the scalp projects in any folder. ‘.scp’ files do not have to be sorted or saved per PK.

#### Analyse projects
`fracsuite scalp transform [folder]`

This transforms the scalp projects into the fracsuite structure so that they can simply be copied into the database using a file explorer. The output directory is `out` in `[folder]`.

At the end of the command, the calculated principal stresses at each measuring point are printed.


### Fracture Images
The specimen folder must be in the default path, with the subdirectories ‘fracture’, ‘acceleration’ and ‘scalp’. The corresponding data is placed in the folders.

If this is to be done automatically, `fracsuite specimen create [specimen-name]` can be used to generate the correct folder structure automatically.

#### Simple method

```pwsh
fracsuite specimen import-fracture [PK] --realsize 500 500 --imgsize 4000 4000
```

This command combines all the steps listed below into one.

#### Sequence
##### Crop the scan
```pwsh
fracsuite splinters crop-frac --specimen-name [specimen] --rotate
```

This command crops the scan to the disc. If the disc is always aligned in the same way when scanning (x-axis of the disc = sliding direction of the 
scanner), `--rotate` must be specified so that the image is correctly aligned.

##### Set preprocessing parameters
```
fracsuite tester threshold [specimen]
```

The preprocessing settings are then checked. If everything is looking good, save the settings. Although this step defines the quality of the detection algorithm, the settings are not that crucial to the analysis success. You can try it out and if the end-result doesn't look good, you can restart the process from this step.

##### Analyse splitter
```
fracsuite gen [specimen] --realsize [real_x] [real_y]
```

Analyse the fracture scan. **Important**: Realsize specifies the actual size in millimetres. This value must be transferred so that the subsequent conversions to mm are correct.

##### Check
```pwsh
fracsuite splinters draw-contours [specimen]
```

Draws the splinter contours found in the scan and displays them for checking purposes.