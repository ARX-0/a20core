# a2o Core

## a2o Core Compliance and Workflow Toward Power ISA 3.1 Compliance

This document outlines the required changes and workflow to make the IBM a2o core compliant with Power ISA 3.1, focusing first on **Book I (User Mode)** additions.

---

## Book I (User Mode) Changes

### 1. New Branch Instructions in Power ISA v3.1

The following new branch instructions are added in Power ISA v3.1:

- `brd`
- `brh`
- `brw`

These are not present in the original a2o core (Power ISA v2.07B).

![brd-brh-brw](https://github.com/user-attachments/assets/1298b55b-eb70-418d-bae6-6b4dc41084b9)
![brd-brh-brw-2](https://github.com/user-attachments/assets/03c39d81-4819-43f1-83d8-37461c79ba5b)
![brd-brh-brw-3](https://github.com/user-attachments/assets/9f83c777-c713-4761-8505-aa2df1b73612)

Reference: current instruction set from `2.07B.pdf` in the a2o repo.

![207b-branch-ref](https://github.com/user-attachments/assets/93c52352-cd18-4029-8727-ba9e16a42fe8)

---

### 2. Bit Manipulation Instructions

Newly introduced in Power ISA v3.1:

- `cfuged`
- `pdepd`
- `pextd`

These are absent in the a2o core's 2.07B instruction set.

![cfuged](https://github.com/user-attachments/assets/77177e62-369b-4226-ba8e-10811d57cdc8)
![pdepd](https://github.com/user-attachments/assets/f6f208df-a11b-4618-9b85-8363c44ebffb)
![pextd](https://github.com/user-attachments/assets/3889e256-754b-41bb-bb5b-0b061c2f44d0)

Reference from the current a2o core:

![207b-bitmanip-ref](https://github.com/user-attachments/assets/51efdfd2-d2e9-45de-8f8a-35db2d7e5c67)

---

### 3. Count Leading/Trailing Zero Instructions

New instructions in v3.1:

- `cntlzdm`
- `cnttzdm`

These are not implemented in a2o.

![cntlzdm](https://github.com/user-attachments/assets/8c98bbc8-5e1f-46a1-b648-6d603f0f3e11)
![cnttzdm](https://github.com/user-attachments/assets/accdd746-1d06-43cd-8935-e20c8b97d6eb)

Reference from a2o repo:

![207b-cntlz-ref](https://github.com/user-attachments/assets/51efdfd2-d2e9-45de-8f8a-35db2d7e5c67)

---

## Divergence Point: Prefixed Instructions

### Prefixed Load and Store Instructions (Pending Clarification)

Power ISA v3.1 introduces instructions like:

- `plbz`, `plhz`, `plha`, `plwz`, `plwa`, `pld`, `plq`, `plfd`, `plfs`
- `pstb`, `psth`, `pstw`, `pstd`, `pstq`, `pstfd`, `pstfs`

The D-form instruction format appears **unchanged** between Power ISA 2.07 and 3.1.

![dform-31](https://github.com/user-attachments/assets/dee6fb8c-8556-44d8-a9a9-e2fae69554b0)
![dform-comp1](https://github.com/user-attachments/assets/fd8ab9d6-02a8-4b47-bd78-6f4b0ee9a711)
![dform-comp2](https://github.com/user-attachments/assets/7403953c-c2c7-44db-ad4b-03568ce5a5dd)

**Pending:** Clarification on the need for both prefix and suffix despite identical D-forms.

---

## Conditional Register Bit Set Instructions

### `setb` and `setbcr`

- `setbr` is not found in v3.1.
- `setb` uses X-form with the BFA field (targets CR or FPSCR).

#### Power ISA v3.1 `setb` (pg 129):

![setb-31](https://github.com/user-attachments/assets/88364063-2dad-489a-9a66-dd9a6abff0d7)

#### Power ISA v2.07 `X-form` with BFA field:

![xform-207b](https://github.com/user-attachments/assets/f295c200-3f81-47aa-a892-7d64cc839e47)

#### Power ISA v3.1 `X-form` BFA variant:

![xform-31](https://github.com/user-attachments/assets/01400a43-3289-44d9-bcca-d3ef55687d48)

### `setbcr` (pg 129 v3.1)

![setbcr-1](https://github.com/user-attachments/assets/bae7423e-1adc-4554-b0d5-d997b827abae)
![setbcr-2](https://github.com/user-attachments/assets/f4bcf56f-2f9c-44eb-9165-57b0d96b4a69)
![setbcr-3](https://github.com/user-attachments/assets/dbec0ee7-8265-495e-9710-6d83b6f75ff3)

the 2.07B (pg 15) dosent have the BI feild in the X-form instructions, the 3.1v (pg 13) also dosent have the above format
I am left confused.... 
the BI in the X-form instr type is unheard of ? 

---

## `addex` Instruction

### Power ISA v3.1 (pg 80)

![addex-31](https://github.com/user-attachments/assets/7630f152-751d-41ec-869a-19408526e975)

This appears to be a floating-point-centric addition instruction.

From 2.07B:

![z23-207b](https://github.com/user-attachments/assets/1f785953-8b9e-4d6e-a311-e4aa742b1bf7)

Z23 compliance implies it is within scope, but may need context checks for floating-point vs integer.

---

## Special Floating-Point Control Register Moves

Instructions added in Power ISA v3.1:

- `mffscdrn`
- `mffscdrni`
- `mffsce`
- `mffscrn`
- `mffscrni`
- `mffsl`

Reference (from page 182 onwards):

![mffscdrn](https://github.com/user-attachments/assets/e1decb6a-3550-4d17-bb24-6a47f63e8767)
![mffscdrni](https://github.com/user-attachments/assets/0023cf8f-da0c-404b-a5a3-2e9294749ab6)
![mffsce](https://github.com/user-attachments/assets/33a9f487-6c6e-4626-baf5-b9de8cd607ed)
![mffscrn](https://github.com/user-attachments/assets/627e900d-650e-4bb1-92a1-c555d09facc1)
![mffscrni](https://github.com/user-attachments/assets/baa9920d-d772-4578-8d79-a7fd3224a5e6)
![mffsl](https://github.com/user-attachments/assets/6b482ea9-a217-446b-b53e-b81072a14921)

Comparison from 3.1 ISA:

![fp-3.1-ref-1](https://github.com/user-attachments/assets/9dcc7acc-4c55-46ca-9f98-c165bb3612cc)
![fp-3.1-ref-2](https://github.com/user-attachments/assets/b7a7c4e0-d058-46e0-af03-a2670ad00937)

Legacy 2.07B similarity (not viable for full coverage):

![fp-legacy](https://github.com/user-attachments/assets/5c7f67f4-e5cf-45d4-9655-5169fa1f42e3)

---

