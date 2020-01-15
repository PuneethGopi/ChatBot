Get training data
If you'd like to train your own model, you'll need training data. There are a few options here.

Use pre-formatted Reddit training data. This is what the pre-trained model was trained on.

Download the training data (2.1 GB). Unzip the monolithic zip file. You'll be left with a folder named "reddit" containing 34 files named "output 1.bz2", "output 2.bz2" etc. Do not extract those individual bzip2 files. Instead, place the whole "reddit" folder that contains those files inside the data folder of the repo. The first time you run train.py on this data, it will convert the raw data into numpy tensors, compress them and save them back to disk, which will create files named "data0.npz" through "data34.npz" (as well as a "sizes.pkl" file and a "vocab.pkl" file). This will fill another ~5 GB of disk space, and will take about half an hour to finish.

Generate your own Reddit training data. If you would like to generate training data from raw Reddit archives, download a torrent of Reddit comments from the torrent links listed here. The comments are available in annual archives, and you can download any or all of them (~304 GB compressed in total). Do not extract the individual bzip2 (.bz2) files contained in these archives.

Once you have your raw reddit data, place it in the reddit-parse/reddit_data subdirectory and use the reddit-parse.py script included in the project file to convert them into compressed text files of appropriately formatted conversations. This script chooses qualifying comments (must be under 200 characters, can't contain certain substrings such as 'http://', can't have been posted on certain subreddits) and assembles them into conversations of at least five lines. Coming up with good rules to curate conversations from raw reddit data is more art than science. I encourage you to play around with the parameters in the included parser_config_standard.json file, or to mess around with the parsing script itself, to come up with an interesting data set.

Please be aware that there is a lot of Reddit data included in the torrents. It is very easy to run out of memory or hard drive space. I used the entire archive (~304 GB compressed), and ran the reddit-parse.py script with the configuration I included as the default, which holds a million comments (several GB) in memory at a time, takes about a day to run on the entire archive, and produces 2.1 GB of bzip2-compressed output. When training the model, this raw data will be converted into numpy tensors, compressed, and saved back to disk, which consumes another ~5 GB of hard drive space. I acknowledge that this may be overkill relative to the size of the model.

Provide your own training data. Training data should be one or more newline-delimited text files. Each line of dialogue should begin with "> " and end with a newline. You'll need a lot of it. Several megabytes of uncompressed text is probably the minimum, and even that may not suffice if you want to train a large model. Text can be provided as raw .txt files or as bzip2-compressed (.bz2) files.

Simulate the United States Supreme Court. I've included a corpus of United States Supreme Court oral argument transcripts (2.7 MB compressed) in the project under the data/scotus directory.

Once you have training data in hand (and located in a subdirectory of the data directory):

Train your own model
Train. Use train.py to train the model. The default hyperparameters are the best that I've found, and are what I used to train the pre-trained model for a couple of months. These hyperparameters will just about fill the memory of a GTX 1080 Ti GPU (11 GB of VRAM), so if you have a smaller GPU, you will need to adjust them accordingly (for example, set --num_blocks to 2).

Training can be interrupted with crtl-c at any time, and will immediately save the model when interrupted. Training can be resumed on a saved model and will automatically carry on from where it was interrupted.



Thanks
Thanks to Andrej Karpathy for his char-rnn repo, and to Sherjil Ozair for his TensorFlow port of char-rnn, which this repo is based on.
