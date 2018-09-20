---?image=assets/images/gitpitch-audience.jpg
@title[EDK_II_Porting_Board_pres]
<br><br><br><br><br>
## <span class="gold"   >UEFI & EDK II Training</span>

#### Porting a New Board

<br>
<span style="font-size:0.75em" ><a href='http://www.tianocore.org'>tianocore.org</a></span>
Note:
  PITCHME.md for UEFI / EDK II Training  Porting a New Board Pres

  Copyright (c) 2018, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.



---  
@title[Lesson Objective]
<BR>
### <p align="center"<span class="gold"   >Lesson Objective </span></p><br>

<!---  Add bullets using https://fontawesome.com/cheatsheet certificate
-->
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;Use Existing Platform based on Apollo Lake</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;Clone & Define a the New Board</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Update Hardware Related Changes</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Other Platform Customizations</span> </li>
</ul>

---?image=/assets/images/slides/Slide3.JPG
@title[Open Source Porting Guide]
### <p align="right"><span class="gold" >Open Source Porting Guide</span></p>
<p style="line-height:80%"><span style="font-size:0.8em" >Porting Guide for a new board with the Intel Atom® Processor E3900 Series Platforms (formerly Apollo Lake) Platform <br>
Download <a href="https://firmware.intel.com/sites/default/files/uefi_firmware_porting_guide_for_the_intel_atom_processor_e3900_series.pdf">PDF </a> </span></p>

Note:


---?image=assets/images/binary-strings-black2.jpg
@title[Copy Existing Board Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Copy Existing Board </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Clone existing Board from a current project</span>

---
@title[Defining a new Board]
<br>
<p align="right"><span class="gold" ><b>Defining a new Board</b></span></p>

<p style="line-height:80%"><span style="font-size:0.8em" >Process to add a new platform (“board”) to the existing firmware project, based on a reference design. </span></p>
<p style="line-height:80%"><span style="font-size:0.8em" >The first step is to simply “clone” an existing board and make the appropriate changes </span></p>
<p style="line-height:80%"><span style="font-size:0.8em" >Download Project Source Code – use the Leaf Hill CRB as the starting project </span></p>
<br>
<p style="line-height:60%"><span style="font-size:0.7em" >To download, see latest build instructions:  at <a href="https://firmware.intel.com/projects/IntelAtomProcessorE3900">https://firmware.intel.com/projects/IntelAtomProcessorE3900 </a>  </span></p>

Note:

---
@title[Add a New Board to the Project]
<br>
<p align="right"><span class="gold" ><b>Add a New Board to the Project</b></span></p>
<ul>
<li><span style="font-size:0.8em" >Support for Multiple Boards already available </span></li>
<li><span style="font-size:0.8em" >Many platforms with minor variations can share common code </span></li>
<li><span style="font-size:0.8em" >Board definitions are under the “Board” directory: </span></li>
</ul>
<pre>
```
   edk2-platforms/Platform/BroxtonPlatformPkg/Board

```
</pre>
<p style="line-height:80%"><span style="font-size:0.8em" >Developers can clone the reference project by copying the directory, renaming the directory, and adding references in several configuration files.  </span></p>



Note:


---?image=/assets/images/slides/Slide7.JPG
@title[Copy Existing Board Dir as a Reference]
<p align="right"><span class="gold" ><b>Copy Existing Board Dir as a Reference</b></span></p>
<br>
```
  bash$ cd edk2-platforms/Platform/BroxtonPlatformPkg/Board/
  bash$ cp –R LeafHill NewBoard
```

Note:


---
@title[Copy Existing Board Dir as a Reference]
<p align="right"><span class="gold" ><b>Copy Existing Board Dir as a Reference</b></span></p>
<p style="line-height:80%"><span style="font-size:0.9em" ><b>Changes to Functions, Variables, and File GUIDs </b> </span></p>
<ul style="list-style-type:none">
  <li><span style="font-size:0.8em" >Functions and global variables of libraries under the NewBoard folder need to be changed so they do not conflict with libraries of existing boards. These are referenced in .INF files copied from the reference project: </span></li>
</ul>

```
 BoardInitPreMem.inf
 BoardInitPostMem.inf
 BoardInitDxe.inf

```

<ul style="list-style-type:none">
<li><span style="font-size:0.8em" >UEFI and EDK II associate a globally unique identifier (GUID) with various functions, files, and protocols.  </span></li>
<li><span style="font-size:0.8em" >New file GUIDs must also be generated for .INF files under the NewBoard folder to avoid conflict with GUIDs from existing projects. </span></li>
<li><span style="font-size:0.8em" >Use <a href="https://www.guidgenerator.com/">https://www.guidgenerator.com/ </a>   </span></li>
</ul>

Note:


---
@title[Copy Existing Board Dir as a Reference]
<p align="right"><span class="gold" ><b>Copy Existing Board Dir as a Reference</b></span></p>
<br>
<ul style="list-style-type:none">
 <li><span style="font-size:0.9em" ><b>Add Components to DSC File –  </b></span></li>
   <ul style="list-style-type:none">
     <li><span style="font-size:0.8em" >Add DXE Library </span></li>
     <li><span style="font-size:0.8em" >Add PEI Libraries </span></li>
   </ul>
 <li><span style="font-size:0.9em" ><b>Paths to Binary Stitching Files - Modify the post-build stitch file </b></span></li>
   <ul style="list-style-type:none">
     <li><span style="font-size:0.8em" >Windows – `IFWIStitch_Simple.bat` </span></li>
     <li><span style="font-size:0.8em" >Linux – `edk2-platforms\BuildBIOS.sh` </span></li>
   </ul>
</ul>
<br>
<br>
<span style="font-size:01.0em" ><b>Build and Test with the NEW Board </b> </span>


Note:

---?image=assets/images/binary-strings-black2.jpg
@title[Hardware Related Changes Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Hardware Related Changes </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>


---
@title[Hardware-Related Changes Main]
<p align="right"><span class="gold" ><b>Hardware-Related Changes</b></span></p>
<p style="line-height:80%"><span style="font-size:0.8em" >The next step is editing the cloned project based on the board configuration.  </span></p>
<p style="line-height:80%"><span style="font-size:0.8em" >Update required UEFI IA Firmware changes for custom platforms that vary from a reference hardware design. </span></p>

<ul style="line-height:0.8;"; style="list-style-type:none">
<li><span style="font-size:0.7em" >1. Detection of Board ID & Fab ID </span></li>
<li><span style="font-size:0.7em" >2. Change UART serial port for UEFI IA Firmware debug messages </span></li>
<li><span style="font-size:0.7em" >3. Change system memory parameters </span></li>
<li><span style="font-size:0.7em" >4. Change display devices, peripherals </span></li>
<li><span style="font-size:0.7em" >5. Modify I/O & GPIO configuration </span></li>
<li><span style="font-size:0.7em" >6. Microcode updates </span></li>
</ul>

Note:

---?image=/assets/images/slides/Slide12.JPG
@title[Hardware-Related Changes ACPI]
<p align="right"><span class="gold" ><b>Hardware-Related Changes - ACPI</b></span></p>
<br>
<p style="line-height:80%"><span style="font-size:0.9em" ><b>Add & Remove Peripherals in ACPI SSDT </b> </span></p>
<ul style="list-style-type:none">
 <li><span style="font-size:0.8em" >Add New Peripherals – New Board .ASL file</span></li>
 <li><span style="font-size:0.8em" >Remove Peripherals – Update Global NVS </span></li>
</ul>

Note:

---?image=/assets/images/slides/Slide13.JPG
@title[Hardware-Related Changes Flash Image Tool]
<p align="right"><span class="gold" ><b>Hardware-Related Changes <br>- Flash Image Tool</b></span></p>
<p style="line-height:80%"><span style="font-size:0.9em" ><b>Configuration via Intel® Flash Image Tool</b> </span></p>
<div class="left1">
<ol>
 <li><span style="font-size:0.7em" >Change PMIC/VR config</span></li>
 <li><span style="font-size:0.7em" >Modify I/O Config (PCIe & USB)</span></li>
 <li><span style="font-size:0.7em" >Microcode Updates </span></li>
</ol>
</div>
<div class="right1">
<span style="font-size:0.8em" ></span>
</div>

Note:

---?image=assets/images/binary-strings-black2.jpg
@title[Other Platform Configuration Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Other Platform Configuration </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>


---?image=/assets/images/slides/Slide15.JPG
@title[Other Platform Configuration]
<p align="right"><span class="gold" ><b>Other Platform Configuration</b></span></p>
<div class="left2">
<ul style="line-height:0.8;">
   <li><span style="font-size:0.7em" >Intel® FSP Configuration –  </span></li>
   <ul style="list-style-type:none">
      <li><span style="font-size:0.6em" >Using Intel BCT to Configure Intel FSP Parameters </span></li>
      <li><span style="font-size:0.6em" >Using UPD to Override Intel FSP Parameters </span></li>
  </ul>
  <li><span style="font-size:0.7em" >Change Default Value for User Setup Options </span></li>
  <li><span style="font-size:0.7em" >Custom Boot Logo </span></li>
  <li><span style="font-size:0.7em" >Default Boot Order </span></li>
  <li><span style="font-size:0.7em" >Remove UEFI Shell </span></li>
  <li><span style="font-size:0.7em" >SMBIOS Table </span></li>
  <li><span style="font-size:0.7em" >Signed Capsule Update </span></li>
  <li><span style="font-size:0.7em" >Enabling Verified Boot </span></li>
  <li><span style="font-size:0.7em" >Trusted Platform Module (TPM) </span></li>
  <li><span style="font-size:0.7em" >UEFI Secure Boot </span></li>
  <li><span style="font-size:0.7em" >UEFI Networking </span></li>
 </ul>
</div>
<div class="right1">
<span style="font-size:0.8em" ></span>
</div>

Note:


  
---  
@title[Summary]
<BR>
### <p align="center"><span class="gold"   >Summary </span></p><br>
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;Use Existing Platform based on Apollo Lake</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;Clone & Define a the New Board</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Update Hardware Related Changes</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Other Platform Customizations</span> </li>
</ul>


---?image=assets/images/gitpitch-audience.jpg
@title[Questions]
<br>
![Questions](/assets/images/questions.JPG) 


---?image=assets/images/gitpitch-audience.jpg
@title[Logo Slide]
<br><br><br>
![Logo Slide](/assets/images/TianocoreLogo.png =10x)


---
@title[Acknowledgements]
#### <p align="center"><span class="gold"   >Acknowledgements</span></p>

```c++
/**
Redistribution and use in source (original document form) and 'compiled' forms (converted
to PDF, epub, HTML and other formats) with or without modification, are permitted provided
that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright 
notice, this list of conditions and the following disclaimer as the first lines of this 
file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML
and other formats) must reproduce the above copyright notice, this list of conditions and 
the following disclaimer in the documentation and/or other materials provided with the 
distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED 
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND 
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE 
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES 
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, 
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, 
WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) 
ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY 
OF SUCH DAMAGE.

Copyright (c) 2018, Intel Corporation. All rights reserved.
**/

```
