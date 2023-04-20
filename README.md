# [whisper.cpp](https://github.com/ggerganov/whisper.cpp)

## To run:
```shell
git clone https://github.com/star-bits/whisper.cpp.git

# download model
bash ./models/download-ggml-model.sh base.en
# build
make
# run on a sample audio file
./main -m ./models/ggml-base.en.bin -f samples/jfk.wav

# or, above three steps can be done at once by
make base.en

# to run with confidence color-coding:
./main -m ./models/ggml-base.en.bin -f samples/jfk.wav --print-colors
```

## To convert `.mp3` into `.wav`:
```shell
ffmpeg -i input.mp3 -ar 16000 -ac 1 -c:a pcm_s16le output.wav
```

## To run on a real-time audio input:
```shell
# build
make stream

# run
./stream -m ./models/ggml-base.en.bin -t 8 --step 500 --length 5000
```

## To use models of different size:
```shell
# 125 MB
make tiny.en
# 210 MB
make base.en
# 600 MB	
make small.en
# 1.7 GB
make medium.en
```

## To utilize Core ML:
```shell
pip install ane_transformers openai-whisper coremltools

# to generate Core ML model:
./models/generate-coreml-model.sh base.en
```
This generates the folder `./models/ggml-base.en-encoder.mlmodelc`. 

If encountered by error `xcrun: error: unable to find utility "coremlc", not a developer tool or in PATH`, run:
```shell
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

To build with Core ML support:
```shell
# undo build
make clean
# build
WHISPER_COREML=1 make -j

# run
./main -m ./models/ggml-base.en.bin -f samples/jfk.wav

whisper_init_state: loading Core ML model from './models/ggml-base.en-encoder.mlmodelc'
whisper_init_state: first run on a device may take a while ...
whisper_init_state: Core ML model loaded
```