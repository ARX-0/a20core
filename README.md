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
(all are compliant)

These are not present in the original a2o core (Power ISA v2.07B).

![brd-brh-brw](https://github.com/user-attachments/assets/1298b55b-eb70-418d-bae6-6b4dc41084b9)
![brd-brh-brw-2](https://github.com/user-attachments/assets/03c39d81-4819-43f1-83d8-37461c79ba5b)
![brd-brh-brw-3](https://github.com/user-attachments/assets/9f83c777-c713-4761-8505-aa2df1b73612)

Reference: current instruction set from `2.07B.pdf` in the a2o repo.

![207b-branch-ref](https://github.com/user-attachments/assets/93c52352-cd18-4029-8727-ba9e16a42fe8)

---

### 2. Bit Manipulation Instructions - compliant

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

### 3. Count Leading/Trailing Zero Instructions - compliant

New instructions in v3.1:

- `cntlzdm`
- `cnttzdm`

These are not implemented in a2o.

![cntlzdm](https://github.com/user-attachments/assets/8c98bbc8-5e1f-46a1-b648-6d603f0f3e11)
![cnttzdm](https://github.com/user-attachments/assets/accdd746-1d06-43cd-8935-e20c8b97d6eb)

Reference from a2o repo:

![207b-cntlz-ref](https://github.com/user-attachments/assets/51efdfd2-d2e9-45de-8f8a-35db2d7e5c67)

---

## Divergence Point: Prefixed Instructions - Complete compliance require clear understaning  in the prefix and sufix field

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

### `setb` and `setbcr` - Incompliant

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

## `addex` - Partial compliance (via Floating point registers instead of GPR)

### Power ISA v3.1 (pg 80)

![addex-31](https://github.com/user-attachments/assets/7630f152-751d-41ec-869a-19408526e975)

This appears to be a floating-point-centric addition instruction.

From 2.07B:

![z23-207b](https://github.com/user-attachments/assets/1f785953-8b9e-4d6e-a311-e4aa742b1bf7)

Z23 compliance implies it is within scope, but may need context checks for floating-point vs integer.

---

## Special Floating-Point Control Register Moves - Partially Compliant

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


### `addpcis` (pg 76) — Not Compliant

> The DX-form instruction set is new and not compliant with 2.07v.

![addpcis_1](https://github.com/user-attachments/assets/018412f3-c8f7-4c18-9909-43c42e8fa917)

![addpcis_2](https://github.com/user-attachments/assets/a46a8584-7bfb-4cc1-86d2-8e3a975a0db3)

---

### `cmpeqb` (pg 95) — Partially Compliant

> In 2.07v it uses float, in 3.1v GPR replaces FPR.

![cmpeqb_1](https://github.com/user-attachments/assets/6b05665c-93ed-4171-a83c-60bc75abe7a4)

![cmpeqb_2](https://github.com/user-attachments/assets/dca00fb0-9b13-4adc-8ded-7a52f0cca932)

![cmpeqb_3](https://github.com/user-attachments/assets/db226faf-024a-432f-9a2d-31c9207f0def)

---

### `cmprb` (pg 94) — Fully Compliant

> Complete compliance with the 2.07v X-form.

![cmprb_1](https://github.com/user-attachments/assets/59c972d8-d800-4775-803c-5ea42fd2da75)

![cmprb_2](https://github.com/user-attachments/assets/f7abb2fa-0a3d-4ed4-9d35-209d23eb137f)

---

### `cnttzd` (pg 104) — Fully Compliant

> Offers full compliance with the 2.07v.

![cnttzd_1](https://github.com/user-attachments/assets/bb6e00ed-1628-48d8-b016-3d14a864868f)

![cnttzd_2](https://github.com/user-attachments/assets/3b1318de-7c2f-4892-a0ed-e8f297e7c1e3)

---

### `darn` (pg 86) — Not Compliant

> 2.07v is not compliant, 3.1v version achieves compliance.

![darn_1](https://github.com/user-attachments/assets/485d8d48-0d4b-4f89-af01-88bbef331226)

![darn_2](https://github.com/user-attachments/assets/34f295d6-ef82-45c8-8197-154f55ccacbe)

---

### `extswsli.` (pg 116) — Fully Compliant

> Fully compliant with the 2.07v ISA.

![extswsli_1](https://github.com/user-attachments/assets/c1c7ad3d-df6e-4683-bbe8-c6abb81f726e)

![extswsli_2](https://github.com/user-attachments/assets/8ff7e494-1848-4bb8-8680-a7fc86d3296e)

---

### `maddld`, `maddhd`, `maddhdu` — Not Compliant

> In 2.07v, GPR instead of Vector Registers should be used. Not compliant.

![madd_1](https://github.com/user-attachments/assets/ed7e0030-b19b-402f-8e9c-b2a32fc498db)
![madd_2](https://github.com/user-attachments/assets/564ee6c1-c05d-407d-b778-d1112cb2725e)
![madd_3](https://github.com/user-attachments/assets/85517362-c8bc-44fb-b8d4-542f44e351cb)

![madd_4](https://github.com/user-attachments/assets/6ef267ea-aea5-4e1b-99b9-fba47ac6e03e)

![madd_5](https://github.com/user-attachments/assets/fb09979e-e9e0-43a4-82ed-a0fc59fb7260)

---

### `mcrxrx` (pg 127) — Fully Compliant

> Compliant with 2.07v.

![mcrxrx_1](https://github.com/user-attachments/assets/49b1b2d2-06cb-4c09-b8ba-90835cbd9a8a)

![mcrxrx_2](https://github.com/user-attachments/assets/e3bf0963-1729-4b9f-ba16-f59e1b370662)

---

### `modsd`, `modsw`, `modud`, `moduw` — Fully Compliant

> Fully compliant with 2.07v.

![mod_1](https://github.com/user-attachments/assets/73fce386-3c71-48c8-b7be-2590d98137b6)

![mod_2](https://github.com/user-attachments/assets/86f699e5-154b-4db9-b2e1-50ccff9b4414)

![mod_3](https://github.com/user-attachments/assets/4bcc9731-1acb-4e92-b2f5-687699d15873)

---

### `scv` (pg 47) — Fully Compliant

> 2.0v compliant `sc` instruction variant.

![scv_1](https://github.com/user-attachments/assets/9d7c6a1d-2dca-459a-a317-cbbbd1155428)

![scv_2](https://github.com/user-attachments/assets/232a118c-2233-4a3c-b6b1-746f2d64059b)

---

### `setb` (pg 129) — Not Compliant

> 2.07v is incompliant. 3.1v has a similar instruction but is not equivalent.

![setb_1](https://github.com/user-attachments/assets/cb70e829-2316-4913-af8d-a45d32806959)

![setb_2](https://github.com/user-attachments/assets/044b3982-76fe-4b2a-b030-d5029a991122)



