# Molecular and Genomic Approaches: Bacterial Meningitis Diagnosis and Surveillance in Africa

28 September – 2 October 2026, 
Centre Suisse de Recherches Scientifiques (CSRS), Côte d'Ivoire

## Topic: Web-based genomics

**Instructors: Dr. Keith Jolley and Prof. Martin Maiden**

## Learning outcomes

Participants will:
* Gain an understanding of sequence-based typing.
* Learn to use online typing databases.
* Practice using web-based genomic epidemiology tools.

## Practical 1: Introduction to molecular sequence typing using PubMLST

PubMLST.org (https://pubmlst.org) is a free online web-based resource that supports sequence typing nomenclature as well as containing extensive isolate and genome libraries.

In this practical we will be using the PubMLST *Neisseria* database to identify MLST alleles, sequence types (STs), and the finetyping antigen peptide (PorA and FetA) variants, for a set of capsular group X meningococcal isolates from Africa. Sequence data for these can be found at https://github.com/WCSCourses/Bacterial_Meningitis_Africa_2026/tree/main/course_data_2026/sequence_typing.

First download and open the Excel worksheet (https://github.com/WCSCourses/Bacterial_Meningitis_Africa_2026/tree/main/course_data_2026/sequence_typing/worksheet.xlsx). Fill this in as you go.

If you wish, you can download all the files as a zip archive from https://github.com/WCSCourses/Bacterial_Meningitis_Africa_2026/tree/main/course_data_2026/sequence_typing/data.zip.

> **Please note that you need to register for a PubMLST account to run the queries necessary for this practical. Please see https://pubmlst.org/site-accounts for details.** 

### Navigating PubMLST
Open a new browser window and type: https://pubmlst.org/neisseria

You will see that the home page is divided into 5 sections:

* A	-	An interactive map summarising all *Neisseria* records in PubMLST;
* B -	‘Typing’: a catalogue of genes annotated in the Neisseria species, including the typing genes PorA, FetA, the capsule locus, and MLST house-keeping genes;
* C -	‘Isolate collection’: a repository of all isolates deposited in the database
* D -	‘Genome collection’; and,
* E -	‘Submit’ where users can submit data for curation and storage in PubMLST.

![](images/pubmlst1.png)

Click 'Typing'.

### Querying sequences

You will see a contents page. To query sequences we will be using the 'Single sequence' query. Click this.

![](images/sequence_typing1.png)

Open the first sequence file [01_BuFa1-11.fas](https://github.com/WCSCourses/Bacterial_Meningitis_Africa_2026/tree/main/course_data_2026/01_BuFa1-11.fas). You will see DNA sequence for the MLST loci (*abcZ*, *adk*, *aroE*, *fumC*, *gdh*, *pdhC*, and *pgm*) as well as for the *porA* and *fetA* genes. You can query a single sequence by copy and pasting it into the web form and selecting the locus. Try this for the *abcZ* sequence, selecting this locus in the dropdown box.

> Make sure you select 'abcZ' and not 'abcZ (NEIS1015)' as this is not the gene fragment used in MLST - it is the full length gene used in cgMLST.

![](images/sequence_typing2.png)

Click 'Submit'.

![](images/sequence_typing3.png)

This should have identified the sequence as abcZ-10. Fill this result in your spreadsheet and continue with the other loci. For the PorA results you will need to select 'PorA_VR1' and 'PorA_VR2' loci, and for the FetA results select 'FetA_VR'.

> Note that what is happening behind the scenes here is that the software is creating a BLAST database of known *abcZ* alleles, performing a query of your pasted in sequence against this, and then interpreting the results of the BLAST query for output on the web page.

### Looking up a ST number
When you have a complete MLST profile, you can look up the ST number for it. From the contents page, click the search for allelic profiles 'by allelic profile' link.

![](images/sequence_typing4.png)

Enter the allelic profile in the boxes and click 'Search'.

![](images/sequence_typing5.png)

Enter the result in your spreadsheet.

### Speeding it all up
You have probably found that querying each locus in turn is a bit tedious :-). Fortunately you can paste in the entire sequence file into the search box (it will accept even whole genome assemblies) and select  either 'MLST' or 'Finetyping antigens' in the locus box. This will search either all the MLST loci, or all the finetyping loci in one go and return the results. In the case of MLST, it will even look up the ST number if it finds a complete profile. If you have downloaded the sequences to your local computer you can also drag-and-drop the file rather than pasting.

Try to complete the rest of the spreadsheet. It should now be much quicker to do.

### Questions

1. How many different STs are present in the dataset?
2. Do some profiles appear to be similar to others? - if so what can you tell from this?
3. How many groups of similar STs are there?
4. Is there any geographical signal present in the data?
5. Are the antigens associated with a particular ST always the same?