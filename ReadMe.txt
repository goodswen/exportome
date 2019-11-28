Method #1 – dipeptide amino acid composition
=============================================
Steps:
1.	In directory ‘aa_composition_extract’ (converts protein sequences to machine learning  (ML) input)

	Script: s25_aa_composition.py
	Input file:  babesia_bovis_prot.fasta
	Output file: start_25_aa_composition.txt

	Script: e25_aa_composition.py
	Input file:  babesia_bovis_prot.fasta
	Output file: end_25_aa_composition.txt

	Script: join_start_end.py
	Input files (s):  start_25_aa_composition.txt, end_25_aa_composition.txt
	Output file: T2Bo_aa_composition

2.	In directory ‘select_input’ (extracts ML input except for training proteins)

	Script: select.py
	Input files (s):  training_file.txt, T2Bo_aa_composition
	Output file: T2Bo_aa_composition_input

	Note: ‘training_file.txt’ contains protein IDs to be excluded from output.

3.	In directory ‘select_training’ (extracts ML input for training e.g. positives and negatives)

	Script: select.py
	Input files: positives (or negatives), T2Bo_aa_composition
	Output file: train_pos (or train_neg)

	Note: ‘positives’ and‘negatives’ contain protein IDs for the positive (candidate = Y) and negative (candidate = N) training data.

	Combine train_pos + train_neg into one file called training_profiles (make sure there is only 1 header e.g. ID, SAA, SAR at the top of the file

4.	In directory ‘exportome_aa_composition’ (runs an ensemble of ML algorithms to classify ML input data)

	Script: run_script
	Input file: T2Bo_aa_composition_input (must be located in input directory)
	ML training file: training_profiles (must be located in training_files directory)

	Output file: vaccine_candidates (located in output directory)
	
	Note: Edit ‘run_script’ and change the location of work_dir.
	
Method #2 – Position-Specific Scoring Matrix (PSSM)
====================================================
Steps:
1.	In directory ‘pssm_blast’ (runs psiblast to create PSSMs)

	Script: psiblast_script
	Input file:  protein sequences in fasta format e.g. babesia_bovis_prot.fasta (it is recommended to split the fasta file into a number of separate files matching the number of nodes)

	Output file: a pssm file with the prefix ‘psiblast_’ for each protein in the fasta input. 

	Note: Edit ‘psiblast_script’ and change the location of full_path and fasta_file.

2.	In directory ‘position_psm_extract’ (converts protein sequences to machine learning  (ML) input)

	Script: position_psm_extract.py
	Input file(s):  pssm files with the prefix ‘psiblast’
	Output file: T2Bo_pssm_position


3.	In directory ‘select_input’ (extracts ML input except for training proteins)

	Script: select.py
	Input files (s):  training_file.txt, T2Bo_pssm_position
	Output file: T2Bo_pssm_position_input

	Note: ‘training_file.txt’ contains protein IDs to be excluded from output.

4.	In directory ‘select_training’ (extracts ML input for training e.g. positives and negatives)

	Script: select.py
	Input files: positives (or negatives), T2Bo_pssm_position
	Output file: train_pos (or train_neg)

	Note: ‘positives’ and‘negatives’ contain protein IDs for the positive (candidate = Y) and negative (candidate = N) training data.

	Combine train_pos + train_neg into one file called training_profiles (make sure there is only 1 header e.g. ID, SAA, SAR at the top of the file

5.	In directory ‘exportome_pssm_position’ (runs an ensemble of ML algorithms to classify ML input data)

	Script: run_script

	Note: Edit ‘run_script’ and change the location of work_dir.

	Input file: T2Bo_pssm_position_input (must be located in input directory)
	ML training file: training_profiles (must be located in training_files directory)

	Output file: vaccine_candidates (located in output directory)

Method #3 – subcellular location)
=================================
Steps:
1.	In directory ‘start’ (runs an ensemble of ML algorithms to predict exportome probabilities based on subcellular protein characteristics)

	Script: startup
	Input file:  protein sequences in fasta format e.g. babesia_bovis_prot.fasta (must be located in babesia/proteome directory)

	Output file: vaccine_candidates (located in babesia/proteome directory)

	Note: Edit ‘babesia.ini’ in start/config_dir directory and appropriately change the location of work_dir, email_url and proteome_fasta (and maybe prot_id_prefix, which is related to the ID header in the fasta file e.g. >tr|BBOV_I004950)




