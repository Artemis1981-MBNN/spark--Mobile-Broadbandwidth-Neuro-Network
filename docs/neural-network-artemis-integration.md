# Spark Neural Network Integration with Artemis1981

This document describes the integration between Apache Spark's MLlib Neural Network capabilities and the Jury1981/Artemis1981 repository for mobile broadband neuro network processing.

## Overview

This integration enables seamless connectivity between:
- **Apache Spark MLlib**: Distributed machine learning library with neural network support
- **Neural Network Layer (ANN)**: Feed-forward artificial neural networks for classification and regression
- **Artemis1981**: External repository for enhanced neuro network capabilities

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Apache Spark                             │
│  ┌───────────────────────────────────────────────────────┐  │
│  │               MLlib - Machine Learning                 │  │
│  │  ┌──────────────────────────────────────────────────┐ │  │
│  │  │   Neural Network (ANN) Layer                     │ │  │
│  │  │   - MultilayerPerceptronClassifier               │ │  │
│  │  │   - Feed-Forward Neural Networks                 │ │  │
│  │  │   - Backpropagation Training                     │ │  │
│  │  └──────────────────────────────────────────────────┘ │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            ↕
         Configuration & Data Synchronization
                            ↕
┌─────────────────────────────────────────────────────────────┐
│              Jury1981/Artemis1981 Repository                 │
│         Mobile Broadband Neuro Network Extensions            │
└─────────────────────────────────────────────────────────────┘
```

## Configuration Files

### 1. Neural Network Configuration (`conf/neural-network.conf`)

This file contains Spark-specific settings for neural network operations:

```properties
# Enable Neural Network support
spark.mllib.neuralnetwork.enabled=true

# Default network architecture
spark.mllib.neuralnetwork.defaultLayers=3
spark.mllib.neuralnetwork.defaultActivation=sigmoid

# Training parameters
spark.mllib.neuralnetwork.learningRate=0.01
spark.mllib.neuralnetwork.maxIterations=100

# Mobile Broadband settings
spark.broadband.neuro.enabled=true
spark.broadband.neuro.batchSize=32
spark.broadband.neuro.optimizer=adam
```

### 2. Artemis Integration Properties (`conf/artemis-integration.properties`)

This file configures the connection to the Artemis1981 repository:

```properties
# Repository connection
artemis.repository.url=https://github.com/Jury1981/Artemis1981
artemis.integration.mode=active

# Data synchronization
artemis.data.sync.enabled=true
artemis.data.sync.frequency=300

# Neural network coordination
artemis.neuralnet.coordination=enabled
artemis.neuralnet.dataflow=bidirectional
```

## Usage

### Basic Example

```python
from pyspark.sql import SparkSession
from pyspark.ml.classification import MultilayerPerceptronClassifier

# Create Spark session with Neural Network configuration
spark = SparkSession.builder \
    .appName("NeuralNetworkExample") \
    .config("spark.mllib.neuralnetwork.enabled", "true") \
    .config("spark.artemis.integration.enabled", "true") \
    .getOrCreate()

# Define neural network architecture
layers = [4, 5, 4, 3]  # input, hidden layers, output

# Create and train the model
trainer = MultilayerPerceptronClassifier(
    maxIter=100,
    layers=layers,
    blockSize=128
)

model = trainer.fit(training_data)
predictions = model.transform(test_data)
```

### Running the Integration Example

```bash
# Using spark-submit
./bin/spark-submit \
  --conf spark.mllib.neuralnetwork.enabled=true \
  --conf spark.artemis.integration.enabled=true \
  examples/src/main/python/mllib/neural_network_artemis_integration.py

# Or using configuration files
./bin/spark-submit \
  --properties-file conf/neural-network.conf \
  examples/src/main/python/mllib/neural_network_artemis_integration.py
```

## Neural Network Components

### Available in Spark MLlib

1. **MultilayerPerceptronClassifier**: Feedforward neural network classifier
   - Supports multiple hidden layers
   - Uses backpropagation for training
   - Optimized for distributed computing

2. **Layer Architecture**: Located in `mllib/src/main/scala/org/apache/spark/ml/ann/`
   - `Layer.scala`: Core layer implementation
   - `LossFunction.scala`: Loss functions for training
   - `BreezeUtil.scala`: Numerical operations

## Integration Features

### Mobile Broadband Neuro Network

This integration specifically supports:
- **Distributed Training**: Leverage Spark's distributed computing for large-scale neural networks
- **Real-time Processing**: Stream processing capabilities for continuous data
- **Scalability**: Handle large datasets across cluster nodes
- **Artemis1981 Coordination**: Bidirectional data flow with external repository

### Configuration Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `spark.mllib.neuralnetwork.enabled` | Enable neural network support | false |
| `spark.mllib.neuralnetwork.defaultLayers` | Default number of layers | 3 |
| `spark.mllib.neuralnetwork.learningRate` | Learning rate for training | 0.01 |
| `spark.broadband.neuro.batchSize` | Batch size for training | 32 |
| `spark.artemis.integration.enabled` | Enable Artemis integration | false |
| `artemis.data.sync.frequency` | Sync interval in seconds | 300 |

## Performance Tuning

For optimal neural network performance:

```properties
# Increase memory for neural network operations
spark.neuralnet.executor.memory=4g
spark.neuralnet.driver.memory=2g

# Use Kryo serialization for better performance
spark.neuralnet.serializer=org.apache.spark.serializer.KryoSerializer

# Adjust batch size based on data size
spark.broadband.neuro.batchSize=64
```

## Troubleshooting

### Common Issues

1. **Memory Issues**: Increase executor and driver memory
2. **Slow Training**: Adjust batch size and learning rate
3. **Connection Issues**: Verify Artemis repository URL and network access

### Verification

To verify the integration is working:

```python
# Check configuration
print(spark.conf.get('spark.mllib.neuralnetwork.enabled'))
print(spark.conf.get('spark.artemis.integration.enabled'))
```

## References

- [Apache Spark MLlib Documentation](https://spark.apache.org/docs/latest/ml-guide.html)
- [Spark Neural Network (ANN) Layer](https://github.com/apache/spark/tree/master/mllib/src/main/scala/org/apache/spark/ml/ann)
- [Jury1981/Artemis1981 Repository](https://github.com/Jury1981/Artemis1981)

## License

Licensed under the Apache License, Version 2.0. See LICENSE file for details.
