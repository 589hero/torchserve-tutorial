# torchserve-tutorial
This project is for serving with torchserve with MNIST Dataset.
The whole process is below.

## 1. Train model
```
python train.py
```
Trained model will be saved ```--model_save_path``` argument.

## 2. Archive saved model
```
torch-model-archiver --model-name mnist --version 1.0 --model-file model.py --serialized-file model/mnist_model.pth --handler handler.py --export-path model_store --extra-files utils.py
```
Archived model will be saved ```model/mnist_model.pth```.

## 3. Serve archived model
```
torchserve --start --ncs --model-store=model_store --models=mnist.mar
```
The model will be served in local.

## 4. Use API
### Health check API
```
curl http://localhost:8080/ping
```
### Inference
```
curl http://localhost:8080/predictions/mnist -T <image PATH>
```
The result will be like below.
```
{
  "0": 6.043713437975384e-05,
  "1": 2.3971939299372025e-05,
  "2": 0.00014931938494555652,
  "3": 0.9784474968910217,
  "4": 1.3182818747736746e-06,
  "5": 0.0008604488102719188,
  "6": 1.7805990637498326e-07,
  "7": 5.3782514441991225e-05,
  "8": 0.010620682500302792,
  "9": 0.009782304987311363
}
```
### Describe models
```
curl http://localhsot:8081/models
```
The result will be like below.
```
{
  "models": [
    {
      "modelName": "mnist",
      "modelUrl": "mnist.mar"
    }
  ]
}
```

## Reference
- <a href="https://pytorch.org/serve/">Torchserve Document</a>
