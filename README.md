# Capstone

# Model Requirements and Installation

# Process Steps 

1. Extraction of brake literature from the Wikipedia, cleaning up of the text, resolve coreferences and split paragarmes in the sentenses and generate a 
dataframe

        Scitpt - navigating_wiki_categories.ipynb 
        
        Input file – None

        Output file - Capstone/data/wiki_brake_all_pages.csv


2. Resoolve the coreferneces using neural coref 

        Scitpt - neural_coref.ipynb         
        Input file – Capstone/data/wiki_brake_all_pages.csv
        Output file - Capstone/data/ wiki_brake_all_pages_with_coref_df.csv

3.  a.	Manual extraction of terms, normalization and computatioin of the precision
    b.	Determine the ideal threshold for annotation and terms extraction 
    c.	Using Ideal threshold value extract the terms	.	

        Scitpt -  term_extraction_threshold_determination.ipynb          
        Input file – Capstone/data/wiki_brake_all_pages_with_coref_df.csv
        Output file - /Capstone/data/dec_07_mapping_data_wiki_text_0.8.csv

4.  a. Training Data – From the entities extracted with paired up, terms, get the random sample of 10K files are extracted and are manually mapped with the relationships.  The relationships extracted with wikifier trained openNRE model relations ships are not giving meaningful relationships thought they establish the relationships.
    b. The traned Model is used for predicting the relatioship for the term pairs generated in the above steop   	

        Scitpt -  term_extraction_threshold_determination.ipynb          
        Input file – /Capstone/data/train_data/wiki_model_input_trial_10K.csv - For training the model
                   - /Capstone/data/dec_07_mapping_data_wiki_text_0.8.csv - Input the trained for prediction of relationships
        Output file - 
                a. /Capstone/data/finalized_model.pkl
                b. /Capstone/data/with_train_test_mapping_data_req_text_0.8_results.csv

4. Network Gneration

        Scitpt -  term_extraction_threshold_determination.ipynb          
        Input file – Capstone/data/wiki_brake_all_pages_with_coref_df.csv
        Output file - /Capstone/data/dec_07_mapping_data_wiki_text_0.8.csv
        
5. 9000 entity pairs with 90% of relationship scores from full term pairs
6. Entity Type (for classification/ontology purpose) – Manual annotation for ~1000 entities from above pairs entity_vocab.csv
7. Data integration – Sanitized program Bill of Material
8. Integration – Note book – Input file relation type predicted, bom file.   Output – The file establishing link through CPSC – CSV file
9. Network 
