## Cibersortx pipeline

### Step 1 Signature matrix
**IF** each column of 'single cell reference matrix' is a phenotype label, proceed to step 2   
```
<From cibersortx website>  
The reference sample file is an input file required for custom signature genes file generation by CIBERSORT and consists of a table of the gene expression profiles of reference sample cell populations that will be compared to each other as defined in the phenotype classes file (next section) to generate the custom signature genes file. Each column corresponds to the gene expression profile of a reference sample. Multiple replicates of a given reference sample cell type (one in each column) may be used.'
```

**ELSE** 
take the following two files and convert them using 'convert_reference_file.py'
	single cell reference matrix 
	class matrix  
  
```python
python convert_reference_file.py MCA_liver_cell_expression.tsv MCA_liver_cell_class.tsv
```
### Step 2 Run Cibersortx to create signature matrix  
\:star: parameters (pro tem)  
Single cell input options:    
	Min. Expression 0.5
:exclamation: Download output signature matrix!


### Step 3 Filter Mixure Matrix  
Filter out columns with no expressions, using the signature matrix from step 2
  
```python
python filter_by_signature_genes.py signature_gene_file.txt mixture_file.tsv
```
This creates a new mixture file named {mixture_file}_filterd.tsv

### Step 4 Run Cibersortx to impute cell fractions  
~ Memo~  
For the mouse liver data set, 19133+ samples exceeded the 'Allowed memory size of 536870912 bytes'  
Use something like the following command to divide data set  

```bash
# first 10000 samples
cat mixture_file_filtered.tsv  |  cut -f 1-10001  > mixture_file_filtered_1.tsv
# the rest of the samples (if <10000); first column == gene names (index)
cat mixture_file_filtered.tsv  |  cut -f 1,10002-  > mixture_file_filtered_2.tsv
```

And to check the number of columns per file  
```bash  
```
