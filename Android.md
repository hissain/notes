### Clone part of a Github repo
```
git clone --filter=blob:none --no-checkout https://github.com/tensorflow/examples.git
cd examples
git sparse-checkout init --cone
git checkout master
git sparse-checkout set lite/examples/model_personalization/android
```
