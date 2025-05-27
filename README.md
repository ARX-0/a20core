# a2o Core

## a2o Core Compliance and Workflow Toward Power ISA 3.1 Compliance

This document outlines the required changes and workflow to make the IBM a2o core compliant with Power ISA 3.1, focusing first on **Book I (User Mode)** additions.

---

## Book I (User Mode) Changes

### 1. New Branch Instructions in Power ISA v3.1

The following figures from the Power ISA v3.1 specification show the new branch instructions: `brd`, `brh`, and `brw`.

![image](https://github.com/user-attachments/assets/1298b55b-eb70-418d-bae6-6b4dc41084b9)
![image](https://github.com/user-attachments/assets/03c39d81-4819-43f1-83d8-37461c79ba5b)
![image](https://github.com/user-attachments/assets/9f83c777-c713-4761-8505-aa2df1b73612)

For reference, here is the current instruction set from the `2.07B.pdf` in the a2o core repository:

![image](https://github.com/user-attachments/assets/93c52352-cd18-4029-8727-ba9e16a42fe8)

---

### 2. Bit Manipulation Instructions (`cfuged`, `pdepd`, `pextd`)

These instructions are newly introduced in Power ISA v3.1. Below are the relevant excerpts from the ISA documentation:

![image](https://github.com/user-attachments/assets/77177e62-369b-4226-ba8e-10811d57cdc8)
![image](https://github.com/user-attachments/assets/f6f208df-a11b-4618-9b85-8363c44ebffb)
![image](https://github.com/user-attachments/assets/3889e256-754b-41bb-bb5b-0b061c2f44d0)

The corresponding instruction set from the a2o core’s `2.07B.pdf`:

![image](https://github.com/user-attachments/assets/51efdfd2-d2e9-45de-8f8a-35db2d7e5c67)

---

### 3. Count Leading/Trailing Zero Instructions (`cntlzdm`, `cnttzdm`)

The following instructions were also introduced in v3.1 and are not part of the original a2o core implementation:

![image](https://github.com/user-attachments/assets/8c98bbc8-5e1f-46a1-b648-6d603f0f3e11)
![image](https://github.com/user-attachments/assets/accdd746-1d06-43cd-8935-e20c8b97d6eb)

Current instruction set from the a2o core repo for comparison:

![image](https://github.com/user-attachments/assets/51efdfd2-d2e9-45de-8f8a-35db2d7e5c67)

---

## till this everything is linear and now the curve gets a bit steeper

### Prefixed load and store instructions: plbz, plhz, plha, plwz, plwa, pld, plq, plfd, plfs, pstb, psth, pstw, pstd, pstq, pstfd, pstfs (added in v3.1) {I am leaving this as pending work as i need a bit of clarity}

why is there a prefix and a suffix? 
The D-form instruction is same for the 2.07v and the 3.1v

![image](https://github.com/user-attachments/assets/dee6fb8c-8556-44d8-a9a9-e2fae69554b0)

the D-form instr for the 2.07v and the 3.1v (they both are the same and there are 0 changes)

![image](https://github.com/user-attachments/assets/fd8ab9d6-02a8-4b47-bd78-6f4b0ee9a711)
![image](https://github.com/user-attachments/assets/7403953c-c2c7-44db-ad4b-03568ce5a5dd)

---

### (the setbr is not found in the 3.1v) the setbcr requires a new instr set in the form 

setb pg 129of 3.1v

![image](https://github.com/user-attachments/assets/88364063-2dad-489a-9a66-dd9a6abff0d7)

2.07v X-form with the BFA field {the CR or FPSCR is used as target}

![image](https://github.com/user-attachments/assets/f295c200-3f81-47aa-a892-7d64cc839e47)

3.1v X-form {with the BFA field}

![image](https://github.com/user-attachments/assets/01400a43-3289-44d9-bcca-d3ef55687d48)

---

setbcr pg 129

![image](https://github.com/user-attachments/assets/bae7423e-1adc-4554-b0d5-d997b827abae)
![image](https://github.com/user-attachments/assets/f4bcf56f-2f9c-44eb-9165-57b0d96b4a69)
![image](https://github.com/user-attachments/assets/dbec0ee7-8265-495e-9710-6d83b6f75ff3)

the 2.07B (pg 15) dosent have the BI feild in the X-form instructions, the 3.1v (pg 13) also dosent have the above format
I am left confused.... 
the BI in the X-form instr type is unheard of ? 

--- 
addex pg80

![image](https://github.com/user-attachments/assets/7630f152-751d-41ec-869a-19408526e975)

Z23 of the 2.07 makes it compliant, but all of it in floating point ?...

![image](https://github.com/user-attachments/assets/1f785953-8b9e-4d6e-a311-e4aa742b1bf7)

--

### mffscdrn, mffscdrni, mffsce, mffscrn, mffscrni, mffsl

![image](https://github.com/user-attachments/assets/e1decb6a-3550-4d17-bb24-6a47f63e8767)
![image](https://github.com/user-attachments/assets/0023cf8f-da0c-404b-a5a3-2e9294749ab6)
![image](https://github.com/user-attachments/assets/33a9f487-6c6e-4626-baf5-b9de8cd607ed)
![image](https://github.com/user-attachments/assets/627e900d-650e-4bb1-92a1-c555d09facc1)
![image](https://github.com/user-attachments/assets/baa9920d-d772-4578-8d79-a7fd3224a5e6)
![image](https://github.com/user-attachments/assets/6b482ea9-a217-446b-b53e-b81072a14921)

in the 3.1v isa

![image](https://github.com/user-attachments/assets/9dcc7acc-4c55-46ca-9f98-c165bb3612cc)
![image](https://github.com/user-attachments/assets/b7a7c4e0-d058-46e0-af03-a2670ad00937)


in 20.7v there is a similarity found but its not viable 
![image](https://github.com/user-attachments/assets/5c7f67f4-e5cf-45d4-9655-5169fa1f42e3)



