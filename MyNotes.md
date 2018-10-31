# TensorFlow Serving

TensorFlow serving in production --> https://www.tensorflow.org/serving/

https://www.tensorflow.org/serving/docker

docker pull tensorflow/serving

Upload Model dynamically on the TF Serving:
---------------------------------------------

docker run -p 8501:8501 --mount type=bind,source=/Users/ariyabalasadaiappan/gitariya/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_cpu,target=/models/half_plus_two -e MODEL_NAME=half_plus_two -t tensorflow/serving &

curl -d '{"instances": [1.0, 2.0, 5.0]}'   -X POST http://localhost:8501/v1/models/half_plus_two:predict

{
    "predictions": [2.5, 3.0, 4.5
    ]
}

Building custom container with custom model:
---------------------------------------------
/Users/ariyabalasadaiappan/gitariya/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_cpu

docker run -d --name serving_base tensorflow/serving
docker cp /Users/ariyabalasadaiappan/gitariya/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_cpu serving_base:/models/ariya_half_plus_two
docker commit --change "ENV MODEL_NAME  ariya_half_plus_two" serving_base ariya_serving
docker kill serving_base

POSTMAN: 
URL:    http://localhost:8501/v1/models/ariya_half_plus_two:predict

        {"instances": [1.0, 2.0, 5.0]}

docker run -p 8501:8501 -e MODEL_NAME=ariya_half_plus_two -t ariya_serving &
  

pip install tensorflow-serving-api