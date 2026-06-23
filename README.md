# VZ FZ HS XE OLED Display Replacement

Open source hardware and firmware project for replacing the original display in selected Casio and Hohner instruments with an ESP32 based OLED display solution.

This repository is maintained by Harun Aktas under the DigitalArtDeco name.

## Project status

This is a private open source repair project.

It is not a finished retail product.
It is not a commercial spare part offering.
It is not supplied as a certified consumer product.
No CE marking, conformity assessment, EU declaration of conformity, product certification, warranty or guarantee is provided by the author.

The project exists to document a repair and replacement approach for technically experienced users, builders and repairers.

## Current repository contents

At the moment, this repository may contain some or all of the following files:

```text
README.md
BINARY_INSTALLATION.md
PROJECT_NOTICE_AND_DISCLAIMER.md
LICENSE
LICENSE.hardware
LICENSE.documentation
VZ_FZ_HS_XE_Installation_Guide_1_3.pdf
FW_VZ_FZ_HS-XE_v1/
```

Depending on the current repository revision, firmware source code and hardware design files may still be added later.

Do not describe this repository as a complete product package unless the required design files, source files, documentation and license texts are actually present in the repository.

## What this project does

The project provides a replacement display concept for compatible Casio and Hohner instruments.

The firmware decodes the original display data bus and drives an SSD1306 compatible OLED display using ESP32 based hardware.

The goal is to make old instruments usable again when the original display has become weak, unreadable or mechanically unreliable.

## Compatibility

The installation guide describes use with the following instruments:

```text
Casio VZ-1
Casio VZ-10M
Casio FZ-10M
Casio FZ-20M
Hohner HS-1/E
Hohner HS-2/E
```

The Casio FZ-1 uses a different display form factor and connector layout and is not compatible with this version.

Compatibility should always be checked against the actual instrument before installation. Instruments modified by previous owners may differ from the expected layout.

## Safety warning

Installation requires opening mains powered electronic equipment.

Inside the instrument, hazardous voltages may be present. Capacitors may remain charged after the mains cable has been disconnected.

Installation, modification and testing should only be performed by qualified technical personnel who understand electrical safety, ESD handling, soldering and the risks of working inside mains powered equipment.

Use of this project is entirely at your own risk.

## Installation

For installation details, see:

```text
VZ_FZ_HS_XE_Installation_Guide_1_3.pdf
```

For flashing prebuilt firmware binaries, see:

```text
BINARY_INSTALLATION.md
```

Always check that the firmware file matches the hardware revision and target instrument before flashing.

## Firmware

Prebuilt firmware binaries may be provided for convenience.

Firmware source code, when published in this repository, is licensed under:

```text
GNU General Public License version 3 or later
SPDX-License-Identifier: GPL-3.0-or-later
```

See:

```text
LICENSE
LICENSES/GPL-3.0-or-later.txt
```

If you distribute firmware binaries based on GPL licensed source code, you are responsible for complying with the GPL, including the applicable source code availability obligations.

## Hardware design files

Hardware design files, when published in this repository, are licensed under:

```text
CERN Open Hardware Licence Version 2, Strongly Reciprocal
SPDX-License-Identifier: CERN-OHL-S-2.0
```

See:

```text
LICENSE.hardware
LICENSES/CERN-OHL-S-2.0.txt
```

This may include schematics, PCB layouts, Gerber files, drill files, bill of materials, pick and place files, assembly data and related hardware documentation.

## Documentation

Documentation, installation guides, diagrams, written instructions and project notes are licensed under:

```text
Creative Commons Attribution ShareAlike 4.0 International
SPDX-License-Identifier: CC-BY-SA-4.0
```

See:

```text
LICENSE.documentation
LICENSES/CC-BY-SA-4.0.txt
```

## Commercial and third party use

The open source licenses may allow third parties to study, modify, build, distribute or sell their own versions, provided they comply with the applicable license terms.

However, the author does not manufacture, certify, approve, guarantee or place those third party builds on the market.

Anyone who manufactures, assembles, imports, sells, distributes, installs or otherwise places hardware based on this repository on the market is solely responsible for all legal, technical, safety and regulatory obligations that may apply.

This includes, where applicable:

```text
CE marking
EMC compliance
electrical safety
technical documentation
EU declaration of conformity
product liability
warranty obligations
consumer protection rules
local laws and import rules
```

No one may state or imply that hardware built from this repository is manufactured, certified, approved, guaranteed or sold by Harun Aktas or DigitalArtDeco unless this has been confirmed in writing.

## CE marking and regulatory status

No CE marking is applied or claimed by the author for this repository, the documentation, the firmware, the design files, prototype boards, third party assembled boards, kits, modified instruments or any hardware built by others from these files.

The repository is documentation and source material for a repair project.

It is not a declaration of conformity.
It is not a technical file for a finished product.
It is not an approval for market placement.

Anyone who turns this project into a product, kit, repair service or distributed hardware assembly must make their own legal and technical assessment.

## No warranty

The project files are provided as is and without warranty of any kind.

The author does not guarantee compatibility, reliability, fitness for a particular purpose, regulatory compliance or freedom from errors.

Use, modification, installation, flashing, manufacturing and distribution are entirely at your own risk.

To the maximum extent permitted by law, the author shall not be liable for damage, malfunction, data loss, loss of use, injury, regulatory issues or other consequences arising from the use of this project.

Nothing in this notice excludes liability where such exclusion is not permitted by law.

## No affiliation

This project is not affiliated with, endorsed by, sponsored by or approved by Casio or Hohner.

Casio, Hohner and all other product names, trademarks and company names belong to their respective owners.

Any reference to Casio or Hohner instruments is made only to describe compatibility, repair use and installation context.

## Trademarks and project name

DigitalArtDeco and the name Harun Aktas may not be used to imply endorsement, certification, warranty, commercial supply or approval of third party hardware, kits, services or modified versions.

## Contact

For project related questions:

```text
akhar9119@gmail.com
```

Please note that this is a private repair and open source project. No support obligation, production service or custom manufacturing service is offered.
