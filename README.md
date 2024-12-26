# ACE-AB1
 **Automated Comprehensive Editing and BLAST-based Assignment for AB1 Files**

 [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.14556246.svg)](https://doi.org/10.5281/zenodo.14556246)
 
### Authors:
**Name: Diego Sosa-Reyes**
- ORCID: 0009-0000-9952-5963
- Equal-contrib: true
- Corresponding: digososa@hotmail.com
- Affiliation: "1"

**Name: Gerardo R. Amores**
- ORCID: 0000-0001-7905-6741
- Equal-contrib: true
- Corresponding: true – amoresgr@gmail.com
- Affiliation: "1"

Affiliations:
- Name: Departamento de Ingeniería Genética, Centro de Investigación y de Estudios Avanzados del I.P.N., Campus Guanajuato, AP 629, Irapuato, Guanajuato, 36500, México.
   index: 1
  
Date: 19 November 2024

# Summary
Applied Biosystems sequencing technologies, such as Sanger sequencing, remain a cornerstone of molecular biology due to their rapid, reliable, and cost-effective determination of gene sequences. While sequencing facilities typically provide processed FASTA files, raw AB1 files preserve detailed Phred score data, enabling more precise sequence analysis and quality assessment. Here, we present ACE-AB1, a Python-based script developed to process AB1 files and generate high-quality nucleotide sequences through a systematic and versatile algorithm. For each AB1 file, the algorithm evaluates, compares, and refines a set of candidate sequences using dynamic quality control (QC) criteria, including Phred scores and sequence length thresholds. By applying iterative trimming methods and reprocessing techniques, ACE-AB1 ensures the selection of the highest-quality sequence. This refined sequence is subsequently submitted to the NCBI nucleotide (nt) database via the BLAST tool for accurate gene identification and annotation. ACE-AB1 is implemented on Google Colab, providing universal accessibility through any modern browser on computers, tablets, or smartphones. This cloud-based approach eliminates the need for local installations and computational infrastructure, streamlining workflows for users working with AB1 files. Through seamless Google Drive integration, ACE-AB1 automates the analysis of bulk AB1 files, including Phred score evaluations and BLAST queries, with minimal user input. The tool also generates detailed quality metrics and reports, ensuring transparency and reliability in sequence evaluation. For advanced users, ACE-AB1’s open-source nature allows for the customization of parameters and further optimization, enhancing its versatility across a range of research applications.

# Statement of need
ACE-AB1 provides an efficient and accessible solution for researchers, educators, and laboratories across diverse fields, including molecular biology, microbiology, and bioinformatics. The tool ensures high-quality nucleotide sequence generation, supporting reliable gene identification and annotation.
By eliminating the need for specialized computational resources through its cloud-based implementation, ACE-AB1 is particularly valuable for laboratories with limited infrastructure. Its ability to enforce stringent quality control (QC) criteria minimizes the risk of submitting low-quality sequences to public databases, thereby promoting the integrity of shared genomic data.
For educators, ACE-AB1 serves as an intuitive teaching resource, offering hands-on experience in sequence editing, BLAST analysis, and quality evaluation. Its versatility makes it equally suited to routine laboratory workflows and advanced academic research.

# Significance
ACE-AB1 is a powerful tool that ensures high-quality sequence analysis and reduces the risk of low-quality submissions to public databases. By integrating accessibility, efficiency, and adaptability, the tool supports the integrity of genomic research while democratizing access to reliable sequence processing methods. Its design makes it an indispensable asset for researchers, educators, and laboratories worldwide.

# Implementation
### Overview
ACE-AB1 is a Python script designed for universal accessibility via Google Colab. It offers an automated, user-friendly approach to processing AB1 files, ensuring the generation of high-quality sequences that meet rigorous QC standards.

### Quality Control (QC) Parameters:
Minimum sequence length: 200 nucleotides (L > 200).
Minimum average Phred score: 30 (Q > 30), ensuring 99.9% sequence accuracy.
Important: This is for 16s sequences analysis; these parameters could be changed by the user. 

# Algorithm Workflow

### Initial Candidate Selection:
ACE-AB1 utilizes the Abifpy package to identify an initial sequence using a Phred score cutoff of 0.01. The first 10 nucleotides are evaluated for average Phred score; sequences with an average below 30 are trimmed nucleotide-by-nucleotide until the required threshold is met. The trimmed sequence is then subjected to QC evaluation.

### Sequence Classification:
Sequences failing QC are categorized into three types:
- Type A: L<200, Q>30.
- Type B: L>200, Q<30.
- Type C: L<200, Q<30.

### Type A Recovery:
Back-Walking Approach: The algorithm extends sequence length by trimming from the left side. It walks backward nucleotide-by-nucleotide until a new 10-nucleotide region meets the Phred score threshold.
Adjusted Cutoff: ACE-AB1 reprocesses the AB1 file increasing the Abifpy cutoff values by decreasing the cutoff value by subtracting 0.001 in each run until reaching 0.0001 as the limit.
Evaluation: The candidate sequences are subject to QC parameters. Subsequently, those sequences, passing the QC, are compared and the sequence with higher QC values are retained to further BLAST.

### Type B Recovery:
Trim Site Adjustment: The algorithm adjusts cut sites using nucleotide-by-nucleotide trimming from the left, right, or both sides.
Adjusted Cutoff: ACE-AB1 reprocesses the AB1 file increasing the Abifpy cutoff values by adding 0.01 in each run until reaching 0.2 as the limit.
Evaluation: Idem, candidate sequences generated through these processes are re-evaluated based on the QC criteria, with the highest-quality sequence retained for further analysis.

### Type C Sequences:
Sequences classified as Type C, along with unrecovered Type A and B sequences, are logged with failure details in the final report.

### BLAST Analysis:
Sequences passing QC are submitted to BLAST against the NCBI nt database. Detailed output reports include sequence annotations, script scores, and additional metadata for downstream analyses.

### Workflow

![image](https://github.com/user-attachments/assets/c30fb895-6f2d-47fd-a9d1-09194aa5a625)

FIGURE 1. Implementation insights. The systematic workflow for high-quality sequence evaluation and selection is shown. Boxes in color red are beginnings or endings, black are processes, yellow is the use of data bases, and blue are decisions.

# Running the Script

### System Requirements
ACE-AB1 operates online via Drive and Google Colab and requires an active internet connection. It is compatible with any modern browser on devices including computers, tablets, and smartphones.

### Step-by-Step Instructions

**Prepare Google Drive:**
* Open Google Drive.
* Create two folders named “input” and “output” in your Drive.
* Upload AB1 files to the “input” folder.

**Run the Script:**
* Execute the script by clicking the play button.
* Adjust parameters if needed (e.g., QC thresholds) and proceed with BLAST analysis.

**Retrieve Results:**
* Navigate to the “output” folder in Google Drive.

![image](https://github.com/user-attachments/assets/711615a5-b056-43ee-8083-ee77677584c9)

Figure 2. Script execution process. The AB1 files generated by Capillary Electrophoreses based sequencing platform are loaded into Drive-google at “input” folder. The ACE-AB1 script is downloaded at GitHub (link) and saved at Colab-Google. Analysis is performed by clicking the play button. Results are accessible at the “output” folder from Drive.

# Results include:
A folder with seven text files:
* **BLAST_results:** Sequences passing QC and their BLAST identities.
* **Pass_QC:** High-quality sequences submitted to BLAST.
* **Quality_Scores:** QC metrics for all sequences.
* **Trimmed_sequences:** Analyzed sequences, regardless of QC status.
* **Phred_trimmed_sequence_values:** Phred scores for trimmed sequences.
* **Raw_data_sequences:** Unprocessed sequences.
* **Phred_raw_values:** Phred scores for raw sequences.


# Acknowledgements
We acknowledge contributions from Dra. Gabriela Olmedo-Álvarez, MSc. Africa Islas for their feedback on the applicability and evaluation of the script.
GRA was supported by Postdoctoral Conahcyt fellowship. 

# References
1.	Daisley, B., Vancuren, S. J., Brettingham, D. J., Wilde, J., Renwick, S., Macpherson, C. V., ... & Allen-Vercoe, E. (2024). isolateR: an R package for generating microbial libraries from Sanger sequencing data. Bioinformatics, 40(7), btae448. https://doi.org/10.1093/bioinformatics/btae448.
2.	Boratyn, G. M., Camacho, C., Cooper, P. S., Coulouris, G., Fong, A., Ma, N., ... & Zaretskaya, I. (2013). BLAST: a more efficient report with usability improvements. Nucleic acids research, 41(W1), W29-W33.
3.	Ewing, B., Hillier, L., Wendl, M. C., & Green, P. (1998). Base-calling of automated sequencer traces using Phred. I. Accuracy assessment. Genome research, 8(3), 175-185.
4.	Naik, P., Naik, G., & Patil, M. (2022). Conceptualizing Python in Google COLAB. India: Shashwat Publication.
5.	Arindrarto, W. (2011). abifpy [Software]. Retrieved from https://github.com/bow/abifpy November 22, 2024.
