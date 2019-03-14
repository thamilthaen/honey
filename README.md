# honey
import nltk
import nltk.classify.util
from nltk.classify import NaiveBayesClassifier
from nltk.corpus import names
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
# nltk.download_shell()    -- This is command for downlaoding stopwords and punkt
# download stopwords using shell
# download punkt using shell
 
def word_feats(words):
    return dict([(word, True) for word in words])
 
 
# Reading Keywords from CSV File
with open('D:/new2.csv') as fp:
    Sentence=''
    Word =''
    positive_vocab = []
    negative_vocab = []
    neutral_vocab = []
    cnt_Rec = 0  
    # Looping Records
    for line in fp:
        cnt_Rec= cnt_Rec+1
        line = line.strip()
        print(line)       
        SenWorCat = line.split(",")
        #print(SenWorCat)
        Sentence = SenWorCat[0]
        #print(Sentence)
        Word = SenWorCat[1]
        #print(Word)
        Category = SenWorCat[2]
        #print(Category)
        # Need to remove the first record as header record.
        if(cnt_Rec != 1):       
            if Category == '1': 
                print("positive")
                positive_vocab.append(Word)
            elif Category == '0':
                print("negative")
                negative_vocab.append(Word)
            else:
                print("neutral")
                neutral_vocab.append(Word)   
            
        
positive_features = [(word_feats(pos), 'pos') for pos in positive_vocab]
negative_features = [(word_feats(neg), 'neg') for neg in negative_vocab]
neutral_features = [(word_feats(neu), 'neu') for neu in neutral_vocab]
 
# Hanlding NaiveBayesClassifier Algorithm
train_set = negative_features + positive_features + neutral_features
classifier = NaiveBayesClassifier.train(train_set)
 
# Handling Stop Words
stop_words = set(stopwords.words('english')) 
 
# Predict
neg = 0
pos = 1
posneg=''
sentence = 'I am glad that was taken away from me after it was promised'
sentence = sentence.lower()
words = sentence.split(' ')
for word in words:
    #print(word)
    # Removing the auxillary
    if word not in stop_words:
        print(word)
        classResult = classifier.classify( word_feats(word))
        print(classResult)
        posneg=posneg+classResult  
        
        
        if classResult == 'neg':
            neg = neg + 1
        if classResult == 'pos':
            pos = pos + 1
 
#print('Positive: ' + str(float(pos)/len(words)))
#print('Negative: ' + str(float(neg)/len(words)))


print(posneg)
if(posneg.find('neg')>= 0):
    print('This is negative statement')
else:
    print('This is positive Statement')
