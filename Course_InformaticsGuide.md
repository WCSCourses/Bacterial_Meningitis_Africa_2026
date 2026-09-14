# Molecular and Genomic Approaches: Bacterial Meningitis Diagnosis and Surveillance in Africa Informatics Guide

**Software used during the course**      

## Software used during the course

| Software | Version, if not latest | Module | Notes and installation instructions |
|---|---:|---|---|
| [MEGA](https://www.megasoftware.net/) | 11.0.13 | Bioinformatics | Required. Install `gconf2-common` first:<br>`wget http://archive.ubuntu.com/ubuntu/pool/universe/g/gconf/gconf2-common_3.2.6-6ubuntu1_all.deb`<br>`sudo dpkg -i gconf2-common_3.2.6-6ubuntu1_all.deb`<br><br>Then reinstall `libgconf-2-4`:<br>`sudo dpkg -i libgconf-2-4_3.2.6-6ubuntu1_amd64.deb`<br><br>For command line use:<br>`sudo snap install mega-cmd` |
| [BLAST+](https://blast.ncbi.nlm.nih.gov/doc/blast-help/downloadblastdata.html) | 2.12.0 | Bioinformatics | Required. Install using:<br>`sudo apt update`<br>`sudo apt install ncbi-blast+` |
| [Firefox](https://www.mozilla.org/firefox/) | 135 | Bioinformatics | Required. Used for browser-based course activities. If you dont use Firefox, you can use any other browser that you may have already installed on the laptop |
| [R](https://cran.r-project.org/) | Latest version recommended | R | Required. Download and install R from CRAN.<br><br>After installing R, open R and install the required packages:<br>`install.packages(c("openxlsx", "dplyr", "janitor", "stringi", "ggplot2", "lubridate", "data.table"))` |
| [RStudio](https://posit.co/download/rstudio-desktop/) | Latest version recommended | R | Required. Download and install RStudio Desktop from Posit. You may also use Positron if preferred. |

## Informatics Set-Up
For installation and setup, please refer to the following guides:

- **[Oracle VM VirtualBox Installation Guide](https://github.com/WCSCourses/WCS_Informatics_Guides/blob/main/Installation_Guides/VM_Guide.md)** – Detailed instructions for installing and configuring VirtualBox on different operating systems. *(Note: Separate installations are needed for Intel-based and ARM-based Macs, and the VDI files will differ.)*
- **[Docker Installation Guide](https://github.com/WCSCourses/WCS_Informatics_Guides/blob/main/Installation_Guides/Docker_guide.md)** – Step-by-step guide for installing Docker on Windows, macOS, and Linux.

The Host Operating System Requirements are: <br />
- RAM requirement: 8GB (preferably 12GB) <br />
- Processor requirement: 4 processors (preferably 8) <br />
- Hard disk space: 200GB <br />
- Admin rights to the computer <br />

## Citing and Re-using Course Material

The course data are free to reuse and adapt with appropriate attribution. All course data in these repositories are licensed under the <a rel="license" href="https://creativecommons.org/licenses/by-nc-sa/4.0/">Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)</a>. <a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /> 

Each course landing page is assigned a DOI via Zenodo, providing a stable and citable reference. These DOIs can be found on the respective course landing pages and can be included in CVs or research publications, offering a professional record of the course contributions.

## Interested in attending a course?

Take a look at what courses are coming up at [Wellcome Connecting Science Courses & Conference Website](https://coursesandconferences.wellcomeconnectingscience.org/our-events/).

---

[Wellcome Connecting Science GitHub Home Page](https://github.com/WCSCourses) 

For more information or queries, feel free to contact us via the [Wellcome Connecting Science website](https://coursesandconferences.wellcomeconnectingscience.org).<br /> 
Please find us on socials [Wellcome Connecting Science Linktr](https://linktr.ee/eventswcs)

---
