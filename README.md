# Capstone

The knowledge base associated with each of the life cycles of the product data,  in manufacturing companies are built over many years and have all the characteristics of big data. The typical product life cycle includes market research, concepts creation, product attributes/configurations, product specification to satisfy functional and regulatory/compliance requirements, product design engineering methodologies, Manufacturing, Product warranty support and product in service.
Given the diversity of the People/process/data elements, the data remains in silos with fragile connectivity across the phases which adds complexity, re-work, data duplications, mis representations. These issues can be addressed if the data is harmonized across the product life cycle to enable taxonomy for product data with ontology to identify concepts and their relationships.  Such a representation will aid in establishing the digital connection between the data sources across the life cylce of product with traceability and connections. 
This work deals with the building of knowledge graph from the data across the product lifecycle for the automotive product lifecycle phases. For this study given the high number of parts, a representative sub system is taken up to prove the concept.  The brake substem related product data is considered for this study. For the Brake sub system data the  Wikipedia corpus as well as internal company data have  been used for the costructio of knowledge graphs.    



# Model Requirements and Installation

1. Coreference resolution-- Neuralcoref

The hugging face neural coreference resolution model is used of resolving the coreference. This model takes work embeddings for several words inside and around each mention and features of the mentions like length, location of the mentions which results in a features representation of each mention and it’s surrounding.  This is passed on to the set of neural net to obtain score of each par of mention and possible antecedent. The second neural net gives a score of a mention having no antecedent (possibly the reference to an entity in a text). The model then compares all these scores together and the highest scores to determine where a mention has an antecedent and which one should be.
The text data is divided into sentences and sentences are processed for the resolution of coreferences. This is helpful to extract the information specific to each of the sentences. 





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
