# LipReadingITA

Keras implementation of the method described in the paper 'LipNet: End-to-End Sentence-level Lipreading' by Yannis M. Assael, Brendan Shillingford, Shimon Whiteson, and Nando de Freitas (https://arxiv.org/abs/1611.01599).
Reference - https://github.com/rizkiarm/LipNet


## 1. Requirements
```
python 3.6
cd lipnet pip install -r requirements.txt
zipfile package
scikit image
h5py 2.10.0
nltk
```

## 2. Dataset
In this project we will use an Italian dataset containing the following sentences.


|               Sentence                | ID  |                           Sentence                           | ID  |
|:-------------------------------------:|:---:|:--------------------------------------------------------:|:---:|
|  Salve quanto costa quell' articolo?  |  0  |                   Tutto bene, grazie.                    | 10  |
|     È in offerta, costa 10 euro.      |  1  |                Prendiamo un caffè al bar?                | 11  |
|    Perfetto, vorrei comprarne due.    |  2  |       Certo volentieri, io lo prenderò macchiato.        | 12  |
| Certo ecco a lei, vuole un sacchetto? |  3  |               A che ora arriva il pullman?               | 13  |
|       Sì, grazie e arrivederci.       |  4  |          Dovrebbe arrivare tra qualche minuto.           | 14  |
|     Le auguro una buona giornata.     |  5  |                Quanto costa il biglietto?                | 15  |
|      Buongiorno, io sono Mario.       |  6  | Purtroppo non lo so, però potresti chiedere all’autista. | 16  |
|       Buonasera, io sono Mario.       |  7  |                Va bene, grazie lo stesso.                | 17  |
|       Piacere Luigi, come stai?       |  8  |                          Prego.                          | 18  |
|            Tutto bene, tu?            |  9  |

<br>

### 2.1 Building
For the dataset building, we created a tool to record the videos (https://github.com/BenedettoSimone/Video-Recorder). The videos have a dimension of ``360x288 x 25fps x 4s``. Use the information provided in the repository and on the main page to replicate our work.
<br><br>After gathering the videos for each subject, we organized the dataset with the following structure.

```
DATASET:
├───s1
│   ├─── 0-bs.mpg
│   ├─── 1-bs.mpg
│   └───...
├───s2
│   └───...
└───...
    └───...
```

### 2.2 Forced alignment
Then, we applied for each video the audio and text synchronization (aka forced alignment) using [Aeneas](https://github.com/readbeyond/aeneas). 
<br><br>
After installing Aeneas, we created a copy of dataset and we organized the folder in this way:

```
ForcedAlignment:
│   ├──DatasetCopy:
│       ├───s1
│       │   ├─── 0-bs.mpg
│       │   ├─── 1-bs.mpg
│       │   └───...
│       ├───s2
│       │   └───...
│       └───...
│           └───...
│        
```

Then we followed these steps in the ``terminal``:
1. With the script ``alignment/create_fragments_txt.py``, we created a ``txt`` file for each video following the rules established by Aeneas.
2. With the script ``alignment/autorunAlign.py``, we dinamically created the ``config`` file and we generated the ``align_json`` folder in the ``ForcedAlignment`` folder. 

After running the script the ``ForcedAlignment`` folder will have this structure.

```
ForcedAlignment:
│   ├──DatasetCopy:
│   │   ├───s1
│   │   │   ├─── 0-bs.mpg
│   │   │   ├─── 0-bs.txt
│   │   │   └───...
│   │   ├───s2
│   │       └───...
│   ├──align_json:
│       ├───s1
│       │   ├─── 0-bs.json
│       │   ├─── 1-bs.json
│       │   └───...
│       ├───s2
│       │   └───...   
```


3. As a final step, with the script ``alignment/alignment_converter.py``, we transformed each json file into an ``.align`` file having the following format:
```
0 46000 sil
46000 65000 Perfetto
65000 76000 vorrei
76000 88000 comprarne
88000 92000 due.
92000 99000 sil
```
The first number indicates the start of that word. The second number indicates the stop. Each number represent the frame numbers x 1000. So frames 0-46 are silence, frames 46-65 are the word "Perfetto", etc.

Now we have the ``align`` folder in the ``ForcedAlignment`` folder.

## 3. Mouth extract
Before starting to extract frames and crop the mouth area we insert the ``DATASET`` folder in the project folder and the ``align`` folder in ``Training/datasets/``.

After, we executed the script ``MouthExtract/mouth_extract.py`` that return ``120 frames`` for each video. 

Finally we split the folders in ``Training/datasets/train`` and ``Training/datasets/val`` using 80% for training phase and 20% for validation phase.

# TODO

REDUCE FPS
install ffmpeg exe
install ffmpeg-python package
run change fps


## Developed by
[Simone Benedetto](https://github.com/BenedettoSimone) <br>
[Salerno Daniele](https://github.com/DanieleSalerno)
