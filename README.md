# Knowledge Graph - Connecting the dots to establish the Digital Thread across Product Life Cycle Phases

 The knowledge base associated with each of the life cycles of the product data as it evolves from concepts to final product in use (functional/compliance requirements,  digital design, digital simulation, manufacturing and in use/service) needs to be seamlessly connected digitally so that data remains as single source of truth and are consumed by downstream/upstream phases in context without duplicating or distorting. However this is not the case because the  manufacturing companies have legacy of many years and have  adopted to newer systems specific to domains as per the business requirements. The typical product life cycle includes market research, concepts creation, product attributes/configurations, product specification to satisfy functional and regulatory/compliance requirements, product design engineering methodologies, Manufacturing, Product warranty support and product in service. The data associated with these are huge, diverse and fast growing and have all the characteristics of big data and associated  challenges in processing and generating insights.
Given the diversity of the People/process/data elements, the data remains in silos with fragile connectivity across the phases which adds complexity, results in re-work, data duplications, mis representations. These issues can be addressed if the data is harmonized across the product life cycle to enable taxonomy for product data with ontology to identify concepts and their relationships.  Such a representation will aid in establishing the digital connection between the data sources across the life cycle of product with traceability and connections. 
This work deals with the building of knowledge graph from the data across the product lifecycle for the automotive product lifecycle phases. For this study given the high number of parts, a representative sub system is taken up to prove the concept.  The brake sub-system related product data is considered for this study. For the Brake sub system data the  Wikipedia corpus as well as internal company data have  been used for the construction of knowledge graphs.     


# Model Requirements and Installation

This section provide the dteails of libraries needed to run the code.  

1. Coreference resolution-- Neuralcoref

The hugging face neural coreference resolution model is used of resolving the coreferences of mentions. This model takes the word embeddings for several words inside and around each mention and features of the mentions (length, location of the mentions ) which results features representation of each mention and its surrounding.  This is passed on to the set of neural nets to obtain score  for each pair of mention and possible antecedent. The first neural lay gives score with antecedents and the  second neural net gives  score of a mention having no antecedent (possibly the reference to an entity in a text). The model then compares all these scores together and the highest scores to determine where a mention has an antecedent and which one should be. The Hugginface neuralcoref is used for this study.

   https://github.com/huggingface/neuralcoref
   
   https://spacy.io/universe/project/neuralcoref

 Requirements.txt For Neuralcoref

        1. spacy>=2.1.0,<3.0.0
        
        2. cython>=0.25
        
        3.  pytest
        
        4. neuralcoref

Limiration: The coreference resolution our model (neuralcoref) which works only with python3.7. So used python3.7 and the compatible Spacy (2.1.0) for this. Giving below the setup setps for running the neuralcoref moudle. The soruce code ( neural_coref.ipynb) as well provides the details of setup. For the details and  latest dependecies please refer to the  source https://spacy.io/universe/project/neuralcoref
        
        1. !apt-get install python3.7
        
        2. !pip install spacy==2.1.0
        
        3. !pip install neuralcoref
        
        4. !pip install https://github.com/explosion/spacy-models/releases//download/en_core_web_lg-2.1.0/en_core_web_lg-2.1.0.tar.gz

2. Named Entities (Terms) Extraction 

The coreferences resolved text documents are processed for named entity recognition and extraction.  There are multiple NLP libraries available which annotates the text documents recognizing the parts of speech, or part of text into machine readable elements.  Our text data source is coming from Wikipedia articles so we used wikifier API???s to annotate the entities.
  
  For Extracting annotations interactively:
  http://wikifier.org/
  
  Wikifier API documentation:
  http://wikifier.org/info.html
        

3. OpenNRE model for Entity link prediction

Entity Relationship Extraction - Relationship Labeling for Training the pre-trained OpenNRE BERT Model
For the term pairs extracted, the relationships are extracted using the following OpenNRE models which are trained using the Wikipedia corpus
wiki80_bert_softmax
wiki80_bertentity_softmax

The extracted relationships are analysed and foudn to be too generic. So we took 1000 term pari combinations and annotated with relationship lable using our SME knoweldge in the automotive domain. Using the labelled relationship we created traing data for 70K term paris and used for training the BERT model with entity concatenation

Please refer the openNRE documentation for the details:
https://github.com/thunlp/OpenNRE

 Requirements.txt For openNRE

        1. torch==1.6.0
        
        2. transformers==3.4.0
        
        3. pytest==5.3.2
        
        4. scikit-learn==0.22.1
        
        5. scipy==1.4.1
        
        6. nltk>=3.6.4
        
# Data Folder for downloading the custom trained BERT model and Terms/Annotations Extracted with differnt Threshold values using wikifier API for "Brake" Corpus

      https://drive.google.com/drive/folders/1vGy_Us0rxn_JY6xw6v4NMHX6Z1HjHXtu?usp=sharing
             
             Model File
             - finalized_model.pkl
             Entities
             -  Capstone/data/termsdec_12_mapping_data_wiki_text_0.6.csv
             -  Capstone/data/termsdec_12_mapping_data_wiki_text_0.7.csv
             -  Capstone/data/termsdec_07_mapping_data_wiki_text_0.8.csv
             -  Capstone/data/termsdec_12_mapping_data_wiki_text_0.9.csv
              - Capstone/data/termsdec_12_mapping_data_wiki_text_1.0.csv

# Process Steps - 

Each and Every process steps can be indpendently Executed by ensuring the availability of input files in the data directory. This enables running and validating the process steps independently depending on the Area of Interest.   

1. Extraction of brake literature from the Wikipedia, cleaning up of the text, resolve coreferences and split paragarmes in the sentenses and generate a 
dataframe

        Script - navigating_wiki_categories.ipynb 
        Input file ??? None
        Output file - Capstone/data/wiki_brake_all_pages.csv

2. Resolve the coreferneces using neural coref 

        Script - neural_coref.ipynb         
        Input file ??? Capstone/data/wiki_brake_all_pages.csv  (Output file from the process step #1)
        Output file - Capstone/data/wiki_brake_all_pages_with_coref_df.csv

3.  a. Sample(100 text cells) from coref resolved texts of the fiel wiki_brake_all_pages_with_coref_df.csv).  Using Wikifier API extract terms with mutlple threshold values (0.6, 0.7. 0.8,0.9 and 1.0). The number of terms that are getting extractd is proposional to the threshold value.
    b.	For sample texts also the terms are extracated manually using our subject knowledge expertise and normalized.  
    c.	The normalized manually extracted terms are used as baseline and are compared aginst the terms returned by Wikifier for different thresholds. The threshold value for which the highest score is obtained is taken as an ideas threshold value to be used for the extraction of terms from the full text corpus.  
    d.	Using Ideal threshold value Extract the terms from the text (Capstone/data/wiki_brake_all_pages_with_coref_df.csv) 

        Script -  term_extraction_threshold_determination.ipynb          
        Input file  ??? Capstone/data/wiki_brake_all_pages_with_coref_df.csv  (Output file from the process step #2)
                    - Capstone/data/wiki_entities_baseline.csv - (Random Sampled sentenses with manual extracted terms)
                    
        Output files  - wiki_entities_baseline_threshold_terms.csv - (The file containing random sampled sentenses with terms extracted for different threshold values)
                      - /Capstone/data/dec_07_mapping_data_wiki_text_0.8.csv  (The file containing the terms extracted for all sentenses with the ideal threshold value of 0.8 based precision scores)
                       
                    - The following files have the full details of terms and it's position details in teh input sentenses. These files are uploaded the Gooogle Drive for reference (Due to size limiation not uploaded to git. Also these not required as the terms separeated and teakne up for preicions calcuation are avilable in the file wiki_entities_baseline_threshold_terms.csv)
                      - https://drive.google.com/drive/folders/1vGy_Us0rxn_JY6xw6v4NMHX6Z1HjHXtu?usp=sharing
                     
                     -  Capstone/data/termsdec_12_mapping_data_wiki_text_0.6.csv
                     -  Capstone/data/termsdec_12_mapping_data_wiki_text_0.7.csv
                     -  Capstone/data/termsdec_07_mapping_data_wiki_text_0.8.csv
                     -  Capstone/data/termsdec_12_mapping_data_wiki_text_0.9.csv
                      - Capstone/data/termsdec_12_mapping_data_wiki_text_1.0.csv

4.  a. Training Data ??? The relationships extracted with openNRE models are not giving meaningful relationships though they establish just the relationships. So the model is trained with entity relationship types annotated manually based on our domain expertise. From the entities extracted are paired up(Permutational combinations of entities(terms) for link prediction), the random sample of 10K lines (in the format the BERT Entity prediction modeler can understand) are extracted and are manually mapped with the relationships
    b. The trained Model is used for predicting the relationship for the entity pairs generated in the process steps #3

        Script - train_model.ipynb      
        Input file ??? /Capstone/data/train_data/wiki_model_input_trial_10K.csv - For training the model - Mannually annotated random sample
                   - /Capstone/data/dec_07_mapping_data_wiki_text_0.8.csv (Output from the process step #3) - Input for the trainined model for link prediction
        Output file - 
                a. /Capstone/data/finalized_model.pkl  (Can be downloaded from https://drive.google.com/drive/folders/1vGy_Us0rxn_JY6xw6v4NMHX6Z1HjHXtu?usp=sharing)
                b. /Capstone/data/with_train_test_mapping_data_req_text_0.8_results.csv 
                    
4. Data Integration.ipynb

        Script -   Data Integration.ipynb   
        Input file ???  /capstone/data/bom_sample_brakes.csv
        Output file -  /capstone/data/wiki_bom.csv
        
6. interactive_plot.ipynb

        Script -     interactive_plot.ipynb 
        Input file ???  /capstone/data/model_predictions_wiki_text_0.8_results.csv
        Output file -  NIL

7. Graph Visualizations.ipynb

        Script -     Graph Visualizations.ipynb
        Input file ???  /capstone/data/model_predictions_wiki_text_0.8_results.csv
        Output file -  /capstone/data/wiki_high_score_rels.csv
