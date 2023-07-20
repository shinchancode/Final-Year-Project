# Final-Year-Project

This repo is the source code for implemenation of this [base paper](https://aclanthology.org/2021.findings-acl.199.pdf): *Multi-Lingual Question Generation with Language Agnostic Language Model* 


# [Multilingual Multiple choice Question Generation](https://github.com/shinchancode/Final-Year-Project)

The project focuses on multilingual automatic multiple-choice question generation to develop a robust and efficient system capable of automatically generating high-quality multiple-choice questions in multiple languages. Automating the process of multiple-choice question generation has various benefits. Automation significantly reduces the time and effort required to generate a substantial number of questions, allowing educators and trainers to focus on other essential aspects of teaching and content development.By using predefined rules and algorithms, the system can produce questions that adhere to specific guidelines, styles, and difficulty levels. This consistency helps maintain fairness and reliability in assessments, ensuring that all learners are evaluated on an equal basis.automating the process of multiple-choice question generation brings efficiency, scalability, standardization, customization, and improved learning experiences to educational institutions, trainers, and assessment organizations. It streamlines the question creation process, supports diverse question types and languages, and contributes to fair and effective assessments. Hence , the project aims to create a language agnostic model for creation of multiple choice questions in multiple languages.

### Steps to run this project:
- First open this [![Colab link](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1mwYv1CHU2RV2v0ylYM0EOTIaOXjqfSFK)
- Run all cells, where we do the installation of libraries, train the LSTM model for each language (low level model), perform fine tuning on transformer which is common for all languages (high level model) and run distractor code where we have used WordNet for that.
- To collaborate with front-end using AnVIL software. AnVIL provides a unified platform for creating and sharing data and performance analysis. Anvil Uplink used to link code to Anvil app from anywhere on the Internet Server uplinks make Python code behave like a server module.
- Functions defined in uplink code can be called from the application using server.call
  ### `!pip install anvil-uplink`
- Connection established, application can be executed by providing input and getting MCQs in return.
  
- Now click on the [link](https://colorful-dizzy-sturgeon.anvil.app/) to give input paragraph. (Remember paragraph should be among these languages - English, Hindi, Korean, French and Chinese). And click generate MCQ.
- It will take around 2 minutes to generate MCQs as distractor takes time to formulate and give relatable wrong options.
- **IMPORTANT** - [You will get all resources regarding this project in the Link tab below.](https://github.com/shinchancode/Final-Year-Project#links)


### Download and process the wikidumps

First of all, you should download the wikipedia dumps from [https://dumps.wikimedia.org/](https://dumps.wikimedia.org/ ) , basicly, there are 10 languages used in this paper for pre-training.

|    Language    | Short name | Size |
| :------------: | :--------: | :--: |
|    Chinese     |     zh     | 1.4G |
|    English     |     en     | 14G  |
|     Korean     |     ko     | 679M |
|     French     |     fr     | 4.4G |
|     Hindi      |     hi     | 430M |
|    Burmese     |     bu     | 208M |
|     German     |     de     | 5.8G |
|    Vietnam     |     vi     | 979M |
|    Japanese    |     Ja     | 2.8G |
| Chinese Minnan |     mi     | 124M |

Note that the number of pre-training languages could be larger than the fine-tuning languages.

## System Architecture

- The first step of implementation includes training the LSTM model for five separate languages. The LSTM model implements the basic level language understanding of the language. It is separate for each language. 

- Transformers have thousands of pretrained models where we train it on data specific to our task and it’s called fine tuning. It is used for high level understanding of the subject. The output of LSTM is given as input to one common transformer.

- We have used WordNet to generate distractors for the target questions. It is a large content database that stores many relationships between words. WordNet tags semantic relationships between words.

- For example: Synonyms - car and automobile. It also captures the different meanings of a word. Example: Mouse can refer to an animal or computer mouse. This was the implementation done so far in the backend.

- Talking about the collaboration with the front-end, we use AnVIL software; where AnVIL provides a unified platform for non-computing users and user data scientists to create and share data and performance analysis. We can use Anvil Uplink to link the code to your Anvil app from anywhere on the Internet. Server uplinks make Python code behave like a server module. We can define functions in your uplink code and then use the anvil to call them from our application. server.call. After the connection is established, we can execute our application by giving input and getting MCQ respectively.

![Achitecture](https://github.com/shinchancode/Final-Year-Project/blob/main/Images/architecture.png)

- If you need more insight about the workflow with code snippets and explanation. [VISIT THIS LINK](https://drive.google.com/drive/folders/1Zfh77oRe7QIIOb-bNNQ-5TkVQiIbRtmr?usp=sharing)

## Workflow and Dataflow of the project
The proposed system architecture for generating multilingual multiple-choice questions using LSTM and Transformer models we have some sequence of processes, which includes:

- Input Data: The system takes a multilingual text document as input, containing the content from which questions need to be generated.
  
- Language Preprocessing: The input text is preprocessed to handle language-specific challenges such as tokenization, stemming, and stop-word removal. This step ensures that the text is prepared for further processing in a language-agnostic manner.
  
- Language Understanding (LSTM Model): The preprocessed text is fed into an LSTM (Long ShortTerm Memory) model for language understanding. The LSTM model is trained on multilingual text data to capture the context and semantics of the sentences effectively. The output of the LSTM model represents the learned representations of the input sentences. There is different LSTM for each language.
  
- Question Generation (Transformer Model): The LSTM model's output is then used as input to a Transformer model for question generation. The Transformer model is responsible for generating meaningful and grammatically correct questions based on the input sentences. It consists of an encoder layer to encode the input sentences and a decoder layer to generate questions based on the encoded representations.
  
- Answer Generation: The Transformer model generates a set of candidate answers based on the encoded representations. These candidate answers can be generated by conditioning the decoder on the encoded representations or by using a separate module for answer generation. The candidate answers can include possible correct answers as well as distractors.
  
- Ranking and Selection: The generated candidate answers are ranked based on their relevance to the input sentences. Various techniques, such as similarity measures, can be used to assess the relevance. The top-ranked answer is selected as the correct answer, and the remaining candidates serve as distractors.
  
- Multiple-Choice Options: Distractors can be generated by substituting or altering words in the correct answer or by extracting alternative answers from the input text. The correct answer and distractors are then randomly shuffled to create multiple-choice options.
  
- Iterative Process: Steps 3 to 7 are repeated for each sentence in the input text document to generate multiple-choice questions for the entire document.
  
- Output: The system outputs the generated multiple-choice questions along with the correct answer and distractors, forming a complete set of questions for the multilingual text document. This proposed architecture combines the strengths of LSTM for language understanding and Transformer for question generation, enabling the system to generate multilingual multiple-choice questions accurately and effectively.


## HIGH LEVEL DESIGN OF THE PROJECT

We propose a model that is divided into two modules: the low-level and the high-level module. The overall model is trained for five languages: English, Hindi, Korean, French and Chinese. The low-level module, which is developed individually for each language, implements an LSTM (Long Short-Term Memory) encoder for low-level understanding of each language. The high level module implements the transformer model for higher-level understanding of information and is common for all the languages.

### Use Case Diagram
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/usecase.png" height="350" width="650px" />
</div>
  
### Activity Diagram
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/activity.png" height="350" width="650px" />
</div>
  
### Sequence Diagram
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/sequence.png" height="350" width="650px" />
</div>
  
### Class Diagram
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/class.png" height="350" width="650px" />
</div>

  
To implement the project we used python as our programming language and google colab notebook as platform to run the respective code.
Initially installed basic dependencies such as transformers, sentencepiece, lstm etc and then implemented the different modules of the project. Low-level module:
At first we trained five different LSTM for our five listed languages:

1. English
2. Hindi
3. Korean
4. French
5. Chinese

   Here is the implementation demonstration for each language:
### English
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/english.jpg" height="350" width="650px" />
</div>

### Hindi
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/hindi.jpg" height="350" width="650px" />
</div>

### Korean
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/korean.png" height="350" width="650px" />
</div>

### French
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/french.jpg" height="350" width="650px" />
</div>

### Chinese
<div align="center" >
  <img src="https://github.com/shinchancode/Final-Year-Project/blob/main/Images/chinese.jpg" height="350" width="650px" />
</div>

## Future Scope
- Improving the generator Distractors
- The project can be expanded to support a wider range of languages. By leveraging multilingual NLP techniques and resources, the system can generate MCQs in multiple languages, catering to a broader user base.
- Generating MCQs in a language different from that of the input text.

## Links

Google Colab Link: [Link](https://colab.research.google.com/drive/1mwYv1CHU2RV2v0ylYM0EOTIaOXjqfSFK)

Webpage link: [Link](https://colorful-dizzy-sturgeon.anvil.app/)

Base Paper: [Link](https://drive.google.com/drive/folders/1B8GrV0JG2_IVJ7tocRVAya5IS9MiC_9e)

Paper published at IEEE (In Process): [Link](https://drive.google.com/drive/folders/1qLn1FSsH2YBJwZP7mJp5TQ00uqn1o7bY)

Final Year Project Report: [Link](https://drive.google.com/drive/folders/1e1C0zAe8y7XK-Ro7KnwDwueBWk3xzgXJ)

##

## Literature Survey done regarding this project
| S.No | Title of Paper/Article |
| ------- | --- |
| 1 | [Automatic MCQ Generator Using Natural Language Techniques](https://www.iosrjournals.org/iosr-jce/papers/Vol23-issue4/Ser-1/B2304011115.pdf) |
| 2 | [Question Generator Natural Language Processing](http://cs230.stanford.edu/projects_fall_2020/reports/55771015.pdf)|
| 3 | [Focused Questions and Answer Generation by Key Content Selection](https://ieeexplore.ieee.org/document/9232485) |
| 4 | [Deep learning based Answering Questions using T5 and Structured Question Generation System](https://ieeexplore.ieee.org/document/9788264) |
| 5 | [Studying the usage of Text- To-Text Transfer Transformer to support Code-Related Tasks](https://ieeexplore.ieee.org/document/9401982)|
| 6 | [A Near-Real-Time Answer Discovery for Open-Domain With Unanswerable Questions From the Web](https://ieeexplore.ieee.org/document/9179752) |
| 7 | [An automated multiple choice question generation using natural language processing techniques.](https://www.researchgate.net/publication/351665611_An_Automated_Multiple-Choice_Question_Generation_using_Natural_Language_Processing_Techniques)|
| 8 | [Question Generation for Reading Comprehension of Language Learning Test -A Method using Seq2Seq Approach with Transformer Model](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8959903)|
| 9 | [Question Classification from Thai Sentences by Considering Word Context to Question Generation](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9910313) |
| 10 | [Distractor Generation for Multiple Choice Questions Using Learning to Rank](https://aclanthology.org/W18-0533/)|
| 11 | [Affix-based Distractor Generation for Tamil Multiple Choice Questions using Neural Word Embedding](https://rupkatha.com/V13/n2/v13n216.pdf) |
| 12 | [A Systematic Review of Automatic Question Generation for Educational Purposes](https://www.researchgate.net/publication/337433098_A_Systematic_Review_of_Automatic_Question_Generation_for_Educational_Purposes) |
| 13 | [Question Generation for Reading Comprehension of Language Learning Test](https://ieeexplore.ieee.org/document/8959903)|
| 14 | [Learning to Reuse Distractors to support Multiple Choice Question Generation in Education](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9969921) |
| 15 | [Automatic question generation for subordinate conjunctions of Marathi](https://ieeexplore.ieee.org/document/9785063)|
| 16 | [Automatic Multiple Choice Question Generation From Text : A Survey](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8585151)|
| 17 | [Automatic Distractor Generation for Multiple Choice Questions in Standard Tests](https://aclanthology.org/2020.coling-main.189/)|
| 18 | [Question generation by transformers](https://arxiv.org/pdf/1909.05017v2.pdf)|
| 19 | [A System for Generating Multiple Choice Questions: With a Novel Approach for Sentence Selection](https://aclanthology.org/W15-4410.pdf) |
| 20 | [An Automated Multiple-Choice Question Generation using Natural Language Processing Techniques](https://www.researchgate.net/publication/351665611_An_Automated_Multiple-Choice_Question_Generation_using_Natural_Language_Processing_Techniques)|
| 21 | [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://aclanthology.org/N19-1423.pdf) |
| 22 | [End-to-End generation of Multiple-Choice questions using Text-to-Text transfer Transformer models](https://www.sciencedirect.com/science/article/pii/S0957417422014014) |
| 23 | [Attention Is All You Need](https://proceedings.neurips.cc/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf)|
| 24 | [Automatic question generation and answer assessment: a survey](https://www.researchgate.net/publication/350165545_Automatic_question_generation_and_answer_assessment_a_survey) |
| 25 | [Exploring the Limits of Language Modeling](https://www.researchgate.net/publication/301847993_Exploring_the_Limits_of_Language_Modeling)|
| 26 | [Automated mcq generator using natural language processing](https://www.irjet.net/archives/V8/i5/IRJET-V8I5497.pdf)|
| 27 | [A Survey on Automatic Multiple Choice Questions Generation from Text](https://drive.google.com/drive/folders/11KKUTQ3ES7kOpoze_Vpp1wI90SiIiszp)|
| 28 | [Towards Generalized Methods for Automatic Question Generation in Educational Domains](https://www.cs.cmu.edu/~hn1/papers/ECTEL2022_TowardsGeneralized.pdf)|
| 29 | [Unsupervised multiple-choice question generation for out-of-domain Q&A fine-tuning](https://aclanthology.org/2022.acl-short.83/) |
| 30 | [BERT-based distractor generation for Swedish reading comprehension questions using a small-scale dataset](https://aclanthology.org/2021.inlg-1.43/)|
| 31 | [Automatic Generation of Multiple Choice Questions Using Wikipedia](https://www.researchgate.net/publication/290928426_Automatic_Generation_of_Multiple_Choice_Questions_Using_Wikipedia) |

### Connect with me:

[<img align="left" alt="codeSTACKr.com" width="22px" src="https://img.icons8.com/?size=512&id=n9d0Hm43JCPK&format=png" />][website]
[<img align="left" alt="codeSTACKr | Twitter" width="22px" src="https://img.icons8.com/fluency/48/twitter.png" />][twitter]
[<img align="left" alt="codeSTACKr | LinkedIn" width="22px" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" />][linkedin]
[<img align="left" alt="codeSTACKr | Instagram" width="22px" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/instagram.svg" />][instagram]

<br />

[website]: https://shinchancode.github.io/3d-react-portfolio/
[twitter]: https://twitter.com/CodeShinchan
[instagram]: https://www.instagram.com/aarti.rathiii
[linkedin]: https://www.linkedin.com/in/aarti-rathi-a6031814b/
