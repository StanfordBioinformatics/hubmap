# Getting Started Guide

## General
### Job Responsibilities
The job of the data manager is to collect, validate, and submit data and
metadata, from and on behalf of the researchers, to the data curators at the
consortium, and to work through any issues that come up in the process. While
the core responsibilities are those listed above, because the data manager acts
as an intermediary between two parties, he must also facilitate the resolution
of many issues on either side of the exchange. The responsibilities frequently
overlap with that of project manager, i.e. tracking deadlines, reminding
researchers or consortium staff of their tasks, communicating statuses to
superiors, and so on. This is not officially part of the job, and if you're
lucky, your project manager will handle most of this for you and let you focus
on the data & metadata.

### Institutional Responsibilities
Each member institution of the HuBMAP consortium is responsible for one or more
organ(s); Stanford is doing the large and small intestines in collaboration with
Washington University of Saint Louis (WUSTL). 

## How
### Entities
The important entities to be aware of are:
    `donor -> organ -> organ piece -> sample -> dataset`
Each `->` represents a one-to-many relationship, which means that the number of
entities is increasing from left to right in the lineage shown above. Each
entity has a unique ID – actually, several unique IDs, used according to context
(see [IDs](### IDs)). At last count, there were 18 donors, 25 organs, 83 organ
pieces, 114 samples, and 590 datasets. An "organ piece," by the way, is a
section of the organ, the sigmoid large intestine, for instance, or the duodenum
of the small intestine, which is denoted as a separate link the in the lineage
in HuBMAP.

### IDs
Each entity may be referred to by several different IDs, depending on the
context. The reason for this is that, while Stanford is performing the analysis,
the samples were collected at WUSTL, tracked in HuBMAP's database using a
surrogate key (the "internal" HuBMAP key), presented to the public on HuBMAP's
data portal using a meaningful key (the "external" ID), and the datasets made
available to download via Globus under a universally unique identifier (UUID).

The meaningful pieces of the naming schemes imbued with meaning are as follows:

#### Stanford ID naming scheme
The naming scheme of the Stanford IDs represents the patient ID, collection
source, organ and organ piece, as follows:
1. Patient ID:
  1. First letter is A if the sample was collected at Stanford, B if at WUSTl
  2. Three-digit numeric ID (assigned sequentially and not meaningful)
2. Collection source:
  1. A for autopsy
  2. C for colectomy
  3. E for endoscopy
3. Organ:
  1. 0-2 for small intestine
    1. 0 for mid-jejunum
    2. 0 for ileum
    3. 1 for jejunum
    4. 2 for duodenum
  2. 3-5 for large intestine
    1. 3 for sigmoid
    2. 4 for transverse
    3. 4 for descending
    4. 5 for ascending

Note that the organ and organ piece nomenclature has not been followed
consistently, so don't put too much stock into them. They are additional clues
to use when problem-solving, at best.

#### HuBMAP External ID
The HuBMAP naming scheme represents the institution, donor, organ, organ piece,
and sample number, as follows (Stanford-specific codes only):
1. Institution and donor
  1. STAN for Stanford
  2. Four-digit numeric ID for donor
2. Organ
  1. LI for large intestine; SI for small intestine
3. Organ piece
  1. Large intestine
    1. 1 - ascending
    2. 2 - transverse
    3. 3 - descending
    4. 4 - sigmoid
  2. Small intestine
    1. 1 - duodenum
    2. 2 - proximal jejunum
    3. 3 - mid-jejunum
    4. 4 - ileum
4. Sample number
  1. Sequentially assigned number, no inherent meaning

#### Examples
Donor ID example:
| Institution | Role              | ID                                |
| ----------- | ----------------- | --------------------------------- |
| WUSTL       | Collection        | W111                              |
| Stanford    | Analysis          | B004                              |
| HuBMAP      | External listing  | STAN0007                          |
| HuBMAP      | Internal database | HBM694.QDFG.746                   |
| HuBMAP      | Download URL UUID | (none)                            |

Dataset ID example:
| Institution | Role              | ID                                |
| ----------- | ----------------- | --------------------------------- |
| WUSTL       | Collection        |                                   |
| Stanford    | Analysis          | B004-A-001                        |
| HuBMAP      | External listing  | STAN0007-LI-4-2                   |
| HuBMAP      | Internal database | HBM962.KFBF.598                   |
| HuBMAP      | Download URL UUID | 4ae7fbdc98da4207224202407c8cdee1  |
I track the various IDs in the submission tracker to avoid confusion. The reason 
	
### Metadata
There are several different types of metadata that have to be submitted to the
consortium for a given dataset to be complete:
  - Donor - maintained by Yiing Lin at WUSTL for HuBMAP donors and by Rozelle
      Laquindranum at Stanford for HTAN
  - Sample - maintained by Yiing Lin at WUSTL for HuBMAP donors and by Kristina
      Paul at Stanford
  - Assay - maintained by the researchers performing the assays

The metadata 

### Data
The data submitted to the HuBMAP consortium has to be structured according to 
their requirements, which include directory and file naming conventions and
required directories and files. 

I stage data for submission on the Stanford Center for Genomics supercomputing
cluster (SCG).

HuBMAP - `/oak/stanford/scg/lab_mpsnyder/hubmap`
HTAN - `/oak/stanford/scg/lab_mpsnyder/htan`

The directory tree for each consortium is as follows:
```
  /submissions
    /<year>
      /<assay>
        /as-received
        /as-submitted
        /scripts
        /qc
```
where:
  - `<year>` is the year in which the submission was started (not the year it was finished)
  - `<assay>` is one of:
    - `bulkatacseq`
    - `bulkrnaseq`
    - `codex`
    - `lipidomics`
    - `metabolomics`
    - `proteomics`
    - `snatacseq`
    - `wes`
    - `wgs`

The data from the researchers goes in `as-received` exactly as the researchers
give it to me. It is reorganized in `as-submitted` according to the consortium's
specifications. Keeping a copy of the data *exactly* as it was given to you is
much safer than reorganizing it in place. There will always be a moment when
you question whether you might have accidentally corrupted the data somehow – by
renaming a file improperly, or deleting a file, or overwriting a file and so on
– and having a pristine copy of the original data lets you rest easy.

### Validation
The `ingest-validation` repository has scripts that can be run to validate the
structure and contents of both the data directories and the metadata files.
Doing this before submitting saves time for both you and the data curators at
the consortium.

## Unsolicited Advice
Good record-keeping is essential to succeeeding in this job, and one of the most
useful habits in this regard is leaving bread-crumbs in a variety of places to
jog your memory. Many of the data submissions go on longer than expected, and it
is all too easy to lose track of details. Here are some of the things I do to
stay organized:
    - In email:
      - Be very explicit about *who* will do *what* by *when*
      - Avoid pronouns referring to antecedents more than one paragraph back
      - Add search terms to email subjects and bodies (e.g. IDs, team members, projects)
      - Example: Instead of "Upload the datasets and let me
          know when you're ready to proceed," write "Chiara, please upload the
          CODEX datasets for B009-A-101 and B009-A-201 to SCG under
          `/oak/stanford/scg/lab_mpsnyder/hubmap/submissions/2023/codex/as-received/`
          and fill in the metadata spreadsheet by this Friday (4/28)." 
      - In spreadsheets, add comments and notes when things are not cut and dried
      - In directories, add README files giving context to the contents therein
      - In scripts, add comments explaining the purpose of the script, the
        Sometimes I will quote from an email where someone asked me to do
        something a particular way and copy in, for example, cross-reference
        tables from the email that are used in the script.

The submission tracker.
Keeping track of IDs 

Using OnDemand

putting the 

## What can go wrong
Because the role sits between two parties and depends on both of them equally,
the data manager ends up dealing with problems that come up on both sides.
On the researchers' side:
  - SCG permissions: Not all researchers use SCG or even have accounts on it, so
      you will need to help them get one or use an alternative (like Globus
      configured to use your account as a pass-through to the directory they
      need).
  - Cloud technology failures: Some researchers use 3rd-party technology to
      store their data, and sometimes these platforms aren't easy or fast to get
      the data out of.

## HuBMAP

### Introduction
From the [NIH Common Fund HuBMAP pages](https://commonfund.nih.gov/HuBMAP):
> The goal of the Human BioMolecular Atlas Program (HuBMAP) is to develop an
> open and global platform to map healthy cells in the human body.  In humans,
> the proper functioning of organs and tissues is dependent on the interaction,
> spatial organization, and specialization of all our cells.  Scientists
> estimate there are 37 trillion cells in an adult human body, so determining
> the function and relationship among these cells is a monumental undertaking.
> Using the latest molecular and cellular biology technologies, HuBMAP
> researchers are studying the connections that cells have with each other
> throughout the body.  HuBMAP will build the framework necessary to construct
> the tools, resources, and cell atlases needed to determine how the
> relationships between cells can affect the health of an individual.

### Links

#### Background
- [HuBMAP consortium public site](https://hubmapconsortium.org/)
- [NIH Common Fund HuBMAP pages](https://commonfund.nih.gov/HuBMAP)
- [NATURE 2019: The human body at cellular resolution: the NIH Human Biomolecular Atlas Program](https://rdcu.be/daC4O)
- [Journal articles using HuBMAP data](https://scholar.google.com/citations?user=CtGSN80AAAAJ)
- [Stanford flagship HuBMAP paper](https://docs.google.com/document/d/1xqv6Ofx6ccCowyaNVDyQToXImmPVKDSBmHpz5Zb7Zw0/edit)

#### Functional
- [Submission tracker](https://docs.google.com/spreadsheets/d/1yjCvR10xQQNeCq_sweGueker262u6v-3OFssliIIhOw/edit?pli=1#gid=925390142) (Stanford internal)
- [Sample tracker](https://docs.google.com/spreadsheets/d/1zlPu7fwvVuZ_hdtQ_YJIZwhib5i1BB0TlEF11qgkCco/edit#gid=0) (Stanford internal)
- [Data Portal](https://portal.hubmapconsortium.org/) (both public- and private-facing sides)
- [Ingest Portal](https://ingest.hubmapconsortium.org/) (only private-facing)
- [Globus](https://app.globus.org/file-manager) (only private-facing)
- [HuBMAP | phs002272 | dbGaP | Submission Portal](https://submit.ncbi.nlm.nih.gov/dbgap/31334/)

#### Repositories, etc.
- [Ingest validation repo](https://github.com/hubmapconsortium/ingest-validation-tools) (README.md has lots of good information)
- [Assay metadata specs](https://hubmapconsortium.github.io/ingest-validation-tools/)
- [Assays in depth](https://portal.hubmapconsortium.org/docs/assays)

#### Occasionally used sites
- [HuBMAP protocols (protocols.io)](https://www.protocols.io/workspaces/human-biomolecular-atlas-program-hubmap-method-development/)

HTAN
- Synapse CLI tool vs. AWS CLI




additionallly:
ingest portal search case sensitive
don't use the bulk upload

