# Molecular and Genomic Approaches: Bacterial Meningitis Diagnosis and Surveillance in Africa

28 September – 2 October 2026, 
Centre Suisse de Recherches Scientifiques (CSRS), Côte d'Ivoire

## Topic: Phylogenetics

**Instructors: Prof. Martin Maiden and Dr. Keith Jolley**

## Background

This module aims to introduce participants to the principles of phylogenetic tree construction.

## Learning outcomes

Participants will gain:

* an understanding of the concept of genetic distance and the different models that can be applied to calculate it;
* familiarity with sequence alignment;
* practical experience generating phylogenetic trees

## Exercises

For these exercises we will be using the program MEGA (Molecular Evolutionary Genetics Analysis) which you should install on your laptops. This is a free application that will run on Windows, MacOS or Linux and is available from <https://megasoftware.net/>. The exercises will be run as an interactive session with a talk. Please stop and wait for the next part of the talk when you reach a stop sign:

![](images/stop-sign.png)

### Introduction to phylogenetic analysis

You have been provided with a dataset that consists of the sequences of a gene for a collection of diverse *Neisseria* spp. isolates spanning the known diversity of the genus. You can find these sequences in https://github.com/WCSCourses/Bacterial_Meningitis_Africa_2026/tree/main/course_data_2026/phylogenetics.

Download the files, Neisseria.fas and Neisseria+outgroup.fas, to your desktop. You will be able to drag-and-drop from here into the MEGA window later.

We will be looking at the *rplF* gene that encodes one of the proteins that make up the ribosome, the essential protein factory of the cell. With its essential role we should, therefore, expect it to be relatively conserved within a species, making it a good candidate for phylogenetic analysis.

### Aligning and formatting data
The sequences have been provided in FASTA format. This is the simplest and probably most common format for sequence data. Each sequence within a FASTA file consists of a header line beginning with a ‘>’ character followed by the sequence identifier and optional comments separated by a ‘|’ character. The sequence itself appears on the following line(s) and continues until either the next header line (beginning with a ‘>’) or the end of the file, e.g.

```
>seq_1
TTTGATACTGTTGCCGAAGGTTTGGGCGAAATTCGCGATTTATTGCGCCGTTATCATCAT
GTCAGCCATGAGTTGGAAAATGGTTCGAGTGAGGCCTTATTGAAAGA
>seq_2
TTTGATACCGTTGCCGAAGGTTTGGGTGAAATTCGCGATTTATTGCGCCGTTACCACCGC
GTCGGCCATGAGTTGGAAAACGGTTCGGGTGAGGCTTTGTTGAAAGA
>seq_3
TTTGATACCGTTGCCGAAGGTTTGGGTAAAATTCGCGATTTATTGCGCCGTTACCACCGC
GTCGGTCATGAGTTGGAAAACGGTTCGGGTGAGGCTTTGTTGAAAGA
```
MEGA can read FASTA files. Sequences need to be aligned before they can be used to generate phylogenetic trees.

Run MEGA and open the Neisseria.fas file that you downloaded.

You can either click the ‘Data’ button and then select the file, or simply drag-and-drop the file in to the main interface.

![](images/mega1.png)

A dialog box will ask you whether to ‘Analyze or Align file’ – select ‘Align’.

![](images/mega2.png)

The sequences will be loaded in to the ‘Alignment explorer’ window.

![](images/mega3.png)

Select all the sequences by clicking ``Edit .. Select All``. 

![](images/mega4.png)

Click the Alignment tab and select Align by MUSCLE (codons):

![](images/mega5.png)

Aligning by codon takes account of the fact that these are protein-encoding sequences so the alignment is performed by first translating the codons to amino acids, performing the alignment, and then finally replacing the amino acids with the original codons. This will produce a more robust alignment for coding data than aligning individual nucleotides. 

Alignment options will appear. Leave the default values and click ‘OK’.

![](images/mega6.png)

You will be asked whether you would like to remove gaps before alignment. Click ‘Yes’:

![](images/mega7.png)

Depending on the version of MEGA you are using, you may get a warning that there are stop codons found in the translated sequences. If you do, click 'Ignore' since these are at the ends of the gene and do not affect the alignment.

![](images/stop-sign.png)

### Distances
You can now perform phylogenetic analysis. Click on the ‘Data’ tab and select ‘Phylogenetic Analysis’:

![](images/mega9.png)

The data are protein encoding, so answer ‘Yes’ when asked:

![](images/mega10.png)

The aligned data are available in the main MEGA window.

In order to re-construct a tree from sequence data we need to calculate the genetic distances between each sequence. There are different ways of doing this, employing different evolutionary models. The simplest is the p-distance which is basically a count of the number of differences between two aligned sequences divided by the length of the sequences. Select ‘Distances’ in the main MEGA window and then select ‘Compute Pairwise Distances…’ in the dropdown menu.

![](images/mega11.png)

You may be asked if you want to use the currently active file. Click 'Ok'.

![](images/mega12.png) 

An ‘Analysis Preferences’ dialog will be displayed. Select  ‘p-distance’ in the Model/Method section leaving all other options at their default.

![](images/mega13.png)

Click ‘OK’. A distance matrix will be calculated and displayed. The distance between sequences 1 and 2 (both *N. meningitidis*) is 0.0037453184. This corresponds to 2 nucleotide differences in a total shared length of 534 bases, i.e. (2/534).

![](images/mega14.png)

If you re-calculate distances using a different model, you will see that the values are slightly different. Recalculate using the Jukes-Cantor model. The distance is now 0.0037547012. This small difference is due to a multiple hit correction.

![](images/stop-sign.png)

### Tree building
Now you can generate a Neighbor-Joining tree by selecting ‘Phylogeny’ in the main MEGA window and then ‘Construct/Test Neighbor-Joining Tree’. You may again be asked if you wish to use the currently active data. Click 'Ok' if so.

![](images/mega15.png)

Accept the default options and click ‘OK’.

![](images/mega16.png)

A ‘Tree Explorer’ window will open.

![](images/mega17.png)

The rectangular tree can be misleading because by default it will root at the midpoint whereas the root may not be known. A radiation tree is often a better way to draw an unrooted tree because no assumption of the root is implied. You can show a radiation tree by clicking on the tree icon and selecting ‘Radiation’.

![](images/mega18.png)
![](images/mega19.png)

Switching off the labels will make the tree clearer. You can do this by expanding the 'Tip labels' menu and unchecking the 'Show Taxa Names' checkbox. You can also change the label font size.

Often we will want to explicity root a tree using an outgroup – one or more nodes that we know to be more dissimilar than the other members of the tree. A second dataset has been provided that includes the same dataset with the addition of the *rplF* sequence from a *Kingella kingae* isolate.
Load the Neisseria+outgroup.fas file, align it and generate a Neighbor-joining tree.

![](images/mega20.png)

Now we can see where the root should be, as *Kingella kingae* is the most distant node in the tree. We can explicitly root the tree using this node by selecting the branch from *Kingella kingae* to the other isolates, right-clicking and selecting ‘Root Tree’.

![](images/mega21.png)

The tree is redrawn rooted by the outgroup.

![](images/mega22.png)

![](images/stop-sign.png)

### Bootstrap tests
Bootstrapping is a way of testing the reliability of an inferred tree. It works by randomly replacing a subset of the data and testing whether the topology of a tree generated from these new sequences changes. If it does not then there is a strong signal supporting the topology and we can be more confident of the groupings. The test provides a percentage value for each branch of the tree.

From the MEGA main window, select Phylogeny and Neighbor-joining tree again. This time, in the section marked ‘Phylogeny Test’, select Test of Phylogeny ‘Bootstrap method’ leaving other options at their default settings.

![](images/mega23.png)

The bootstrap values will be displayed on each branch of the tree.

![](images/mega24.png)

### Further reading
The *rplF* gene used in this practical has been used as a reliable target for discriminating different species of *Neisseria* in large-scale carriage studies. More details can be found at:

* Bennett JS, Watkins ER, Jolley KA, Harrison OB, Maiden MCJ 2014. Identifying Neisseria species by use of the 50S ribosomal protein L6 (*rplF*) gene. [*J Clin Microbiol* 52:1375-81](https://pubmed.ncbi.nlm.nih.gov/24523465/).