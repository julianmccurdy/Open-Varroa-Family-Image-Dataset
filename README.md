# Open Varroa Family Image Dataset

Open image dataset for Harbo assay, VSH scoring, Varroa family classification, mite faecal patch detection, and AI-assisted assay workflows.

This project is collecting **labelled and unlabelled images** from Harbo-style Varroa Sensitive Hygiene assessments so the beekeeping, breeding, and research community can build better training material, shared label language, and practical AI tools that assist human scorers during assay work.

The aim is not to replace expert judgement. The aim is to make the Harbo assay faster, more consistent, easier to teach, easier to audit, and eventually suitable for AI-assisted overlays during live examination.

---

## 1. What we are trying to collect

We want images from the **full Harbo assay workflow**, not only final mite-family examples.

Ideal image sets include:

| Stage                           | Image needed                                                            | Why it matters                                                                         |
| ------------------------------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Brood frame / patch context     | Worker brood area before cells are opened                               | Helps document brood age, pattern, and assay context                                   |
| Purple-eye / older pupa in cell | Pupa visible inside the cell before removal                             | Confirms the cell is within the useful Harbo assay age range                           |
| Pupa removed from cell          | Pupa held outside the cell for examination                              | Helps detect foundress mite, mite progeny, damage, brood stage, and false positives    |
| Original cell after removal     | Empty cell wall and floor from the exact cell the pupa came from        | Critical for mite faecal patch, foundress, offspring, and reproductive-family evidence |
| Mite family close-up            | Close image of foundress, eggs, male, nymphs, daughters, or faecal pile | Core AI training target                                                                |
| Negative cell examples          | Valid cells with no mite or no reproductive evidence                    | Essential so AI learns what “nothing” looks like                                       |
| False positives                 | Pollen, wax crumbs, larval skin, glare, mould, debris, damaged cells    | Prevents AI from hallucinating Varroa families                                         |
| Uncertain cases                 | Hard examples where expert review is needed                             | Very valuable for real-world assay assistance                                          |

---

## 2. Minimum useful image set per inspected cell

The best contribution is a **linked set of images from the same cell**.

Minimum useful set:

1. **Cell before pupa removal**
2. **Pupa removed and visible**
3. **Cell wall / floor after pupa removal**
4. Optional close-up of mite, offspring, or faecal patch

Use a shared `cell_id` so the images can be connected.

Example:

```text
NZ2026_Apiary03_Queen17_Frame2_Cell045_before.jpg
NZ2026_Apiary03_Queen17_Frame2_Cell045_pupa.jpg
NZ2026_Apiary03_Queen17_Frame2_Cell045_cellwall.jpg
NZ2026_Apiary03_Queen17_Frame2_Cell045_detail.jpg
```

---

## 3. Image requirements

| Requirement |                                                              Preferred |                          Minimum usable |
| ----------- | ---------------------------------------------------------------------: | --------------------------------------: |
| Resolution  |                                        1600 px or more on longest edge |                 1024 px on longest edge |
| Format      |                                                         JPG, PNG, TIFF |                              JPG or PNG |
| Focus       |                                     Sharp on target cell / mite / pupa |          Mostly sharp and interpretable |
| Lighting    | White light, side light, microscope light, UV/alternate light if noted |    Any usable light if clearly labelled |
| Colour      |                                               Natural colour preferred |                   Avoid extreme filters |
| Framing     |                           One main cell or one clear subject per image |          Wider context is okay if noted |
| Metadata    |                                                     Strongly preferred | Basic submitter + label status required |

Do not crop so tightly that the cell wall or pupa context is lost.

---

## 4. Images that are especially valuable

We need more than beautiful textbook examples.

High-value images include:

| Image type                         | Why valuable                                               |
| ---------------------------------- | ---------------------------------------------------------- |
| Clear reproductive families        | Core positive training examples                            |
| Foundress-only cells               | Helps separate non-reproductive from reproductive cases    |
| Non-viable reproduction            | Important for correct Harbo/VSH interpretation             |
| Multi-foundress cells              | Important edge case                                        |
| Mite poo / faecal patch            | Strong visual cue for reproduction                         |
| Clean no-mite cells                | Essential negative class                                   |
| Larval skin / white debris         | Common false positive                                      |
| Pollen or propolis contamination   | Common false positive                                      |
| Dark old comb                      | Realistic field difficulty                                 |
| Blurry borderline images           | Useful for “not enough confidence” / reject model training |
| Same cell under different lighting | Useful for AI-overlay and lighting optimisation            |

---

## 5. Label set

Initial labels:

| Label                     | Meaning                                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------- |
| `no_mite`                 | No mite or mite family evidence visible                                                      |
| `foundress_only`          | Adult foundress mite visible, no viable reproduction seen                                    |
| `reproductive`            | Foundress plus viable offspring / reproductive evidence                                      |
| `non_reproductive`        | Foundress present but no reproductive offspring                                              |
| `non_viable`              | Reproduction present but not expected to produce viable mature daughter before bee emergence |
| `multi_foundress`         | More than one foundress mite in the cell                                                     |
| `mite_poo_faecal_patch`   | Visible mite faecal deposit / guanine patch                                                  |
| `pupa_damage`             | Damaged or abnormal pupa relevant to scoring                                                 |
| `false_positive`          | Object looks mite-related but is not: pollen, wax, larval skin, debris, glare                |
| `uncertain_expert_review` | Needs expert review                                                                          |
| `unlabelled`              | Submitted for community/expert labelling                                                     |

A single image can have multiple labels.

Example:

```csv
image_id,cell_id,label,confidence,review_status
IMG_0045_cellwall,NZ2026_Q17_F2_C045,mite_poo_faecal_patch,probable,needs_review
IMG_0045_pupa,NZ2026_Q17_F2_C045,foundress_only,certain,reviewed
```

---

## 6. AI overlay targets

This dataset is intended to support future AI-assisted tools during Harbo assay examination.

Possible overlay outputs:

| Overlay target               | Intended use                                                        |
| ---------------------------- | ------------------------------------------------------------------- |
| Cell age / pupa stage prompt | Warn if the cell is too young, too old, damaged, or unclear         |
| Foundress mite candidate     | Highlight likely adult mite                                         |
| Offspring candidate          | Highlight possible egg, male, protonymph, deutonymph, daughter mite |
| Faecal patch candidate       | Highlight mite poo / guanine deposit                                |
| False-positive warning       | Flag likely larval skin, pollen, wax flake, glare, or debris        |
| Confidence score             | Show whether the AI is confident, probable, or uncertain            |
| Suggested label              | Suggest label for human confirmation                                |
| Human correction loop        | Human scorer confirms or corrects the AI label                      |

The model should **assist**, not decide final breeder selection.

---

## 7. Required metadata

Minimum required fields:

```csv
image_id
file_name
cell_id
submission_type
label_status
proposed_label
confidence
lighting_type
device_or_microscope
contributor_name_or_handle
date_submitted
licence
permission_to_share
```

Recommended optional fields:

```csv
country
region
apiary_id_anonymised
hive_id_anonymised
queen_line_id_anonymised
frame_id
brood_stage
pupal_age_estimate
harbo_day_or_days_post_capping
magnification
camera_model
microscope_model
reviewer_name
review_status
notes
```

---

## 8. Suggested metadata values

### submission_type

```text
raw
reviewed
training_candidate
test_candidate
example_only
```

### label_status

```text
labelled
unlabelled
partly_labelled
needs_review
expert_reviewed
```

### confidence

```text
certain
probable
uncertain
unknown
```

### lighting_type

```text
white_diffuse
white_side_light
microscope_ring_light
uv_365
violet_405
infrared
mixed
unknown
```

### brood_stage

```text
purple_eye
older_pupa
young_pupa
adult_near_emergence
unknown
not_applicable
```

---

## 9. Suggested folder structure

```text
open-varroa-family-images/
│
├── README.md
├── CONTRIBUTING.md
├── LICENSE.md
│
├── images/
│   ├── raw/
│   ├── reviewed/
│   ├── examples/
│   └── rejected/
│
├── metadata/
│   ├── metadata.csv
│   ├── labels.csv
│   ├── cells.csv
│   └── contributors.csv
│
├── docs/
│   ├── label-guide.md
│   ├── image-spec.md
│   ├── harbo-workflow.md
│   └── ai-overlay-notes.md
│
└── annotation/
    ├── coco/
    ├── yolo/
    └── roboflow_exports/
```

---

## 10. Suggested `cells.csv`

Use this to connect multiple images from the same inspected cell.

```csv
cell_id,apiary_id,hive_id,queen_line_id,frame_id,cell_number,brood_stage,final_cell_label,review_status,notes
NZ2026_Q17_F2_C045,,,Q17,F2,45,purple_eye,reproductive,expert_reviewed,foundress plus offspring and faecal patch
```

---

## 11. Suggested `labels.csv`

Use this when multiple labels or bounding boxes are needed for one image.

```csv
image_id,cell_id,label,object_type,x_min,y_min,x_max,y_max,confidence,reviewer,review_status,notes
IMG_0045_cellwall,NZ2026_Q17_F2_C045,mite_poo_faecal_patch,region,430,280,520,370,probable,,needs_review,white deposit on rear cell wall
IMG_0045_pupa,NZ2026_Q17_F2_C045,foundress_only,object,610,410,690,490,certain,Rae,expert_reviewed,adult foundress visible
```

Bounding boxes are optional at first. Classification labels are useful immediately.

---

## 12. Not acceptable images

Please avoid submitting:

| Not acceptable                                     | Why                                     |
| -------------------------------------------------- | --------------------------------------- |
| Very blurry images                                 | Cannot train reliable labels            |
| Overexposed or underexposed images                 | Key detail is lost                      |
| Heavy filters or AI-enhanced fake detail           | Corrupts the dataset                    |
| Collages or screenshots                            | Hard to trace and label correctly       |
| Text covering the cell                             | Blocks useful visual information        |
| Duplicate images with no note                      | Pollutes the dataset                    |
| Images without permission to share                 | Cannot be used publicly                 |
| Images where the cell/pupa relationship is unknown | Less useful for Harbo workflow training |

Borderline images can still be submitted if marked as `uncertain` or `poor_quality`.

---

## 13. Licensing

Only submit images you own or have permission to share.

Preferred licence:

```text
Creative Commons Attribution 4.0 International — CC BY 4.0
```

This allows researchers, beekeepers, breeders, educators, and developers to reuse the images with attribution.

If a contributor needs a different licence, note it clearly in the metadata.

---

## 14. Contributor note

You can contribute even if you are not confident in the label.

Unlabelled images are welcome.

Useful submissions include:

* raw assay images
* labelled expert examples
* uncertain cases
* false positives
* poor-quality examples for rejection training
* repeated views of the same cell under different lighting
* images from live Harbo assay workflows

The community can help review, label, and improve the dataset over time.

---

## 15. Contact

For repo access, contribution guide, or to help with labelling:

```text
Contact: Julian McCurdy
Email: julian@buzztech.nz
Project: Open Varroa Family Image Dataset
Focus: Harbo assay, VSH scoring, Varroa family classification, and AI-assisted assay tools
```

Open collaboration for practical Varroa and VSH research.
