- [What is Federated Learning?](#what-is-federated-learning)
    - [Clients](#clients)
    - [Aggregators](#aggregators)
    - [Neighbours](#neighbours)
- [Federated Learning Cycle](#federated-learning-cycle)
  - [Client selection](#client-selection)
  - [Model download](#model-download)
  - [Local training](#local-training)
  - [Reporting](#reporting)
  - [Aggregation](#aggregation)
- [Federated Learning in Deployment](#federated-learning-in-deployment)
  - [Device instrumentation](#device-instrumentation)
  - [Simulation](#simulation)
  - [Federated model training](#federated-model-training)
  - [Federated model evaluation](#federated-model-evaluation)
  - [Deployment](#deployment)

# What is Federated Learning?

Before we dive into how you can deploy an FL system lets go through the lifecycle of federated learning and set the standard definitions. [Kairouz et. al.](https://arxiv.org/pdf/1912.04977.pdf) is a fantastic survey of the current literature on Federated Learning. We quote them to define Federated Learning

>_**Federated learning** is a machine learning setting where multiple entities ([clients](#clients)) collaborate in solving a machine learning problem, under the coordination of a central server or service provider. Each client’s raw data is stored locally and not exchanged or transferred; instead,focused updates intended for immediate aggregation are used to achieve the learning objective._

Types of federated learning:
- Cross silo Federated Learning
- Cross device Federated Learning
- Vertical Federated Learning
 
### Clients
Clients are the devices responsible for training the model on a dataset hold locally. 

### Aggregators
Aggregators are resposible for taking model updates from several clients and generating an averaged model from the submissions.
The same device can behave as both a client and aggregator as it happens in decentralized FL.

### Neighbours
Neighbours in the context of FL are workers (clients/aggregators) which can send and recieve model updates from each other. 
In the standard centralized FL setting, every client has only the central server as its neighbour.

# Federated Learning Cycle
An horizontal FL cycle consists of 5 steps:

## Client selection 
Before we start training our global model, we need to select the participants. Every aggregator samples a subset or all of its neighbours and asks for model updates from it. In some cases the neighbours first apply for participation and then the aggregator decides who amongst them should be accepted.

## Model download
Download the model parameters and execution plans, if not done already. Once accepted into the cycle, the workers ask for necessary information to begin training. 

## Local training
Each worker runs a specific number of iterations locally with the data that is available on the device. It updates its local model weights and uses them for inference

## Reporting
Once all the participants have finished the training process, they submit their model updates to the aggregator which began the cycle. The aggregator keeps on waiting until a fraction of accepted devices report back with their models. 

The workers often only send the compressed model weights to reduce the data consumption

## Aggregation
The aggregator then averages the model weights to generate the final global model. It often uses non-linear combination of these models to account for their history and error in communication.

# Federated Learning in Deployment 
Now lets explore the roles and steps needed to productionize the fl deployment. What will an engineer need to do to deploy these solutions?

## Device instrumentation
Store relevant data with an expiry date that is necessary for training.
Preprocessing data for future use. Storing user-item interaction matrix for recommendations, etc

## Simulation
Prototype model architectures and FL strategies on a dummy data on cloud to set the expectations of the architecture.

## Federated model training
Run training procedures for different types of models with different hyper parameters. At the end we choose the best ones for aggregation.

## Federated model evaluation
Metrics are extracted out on the held out data on cloud and the data distributed on the devices to find the performance.

## Deployment
Manual quality assurance, live A/B testing and staged rollout. Usually the engineer determines this process. It is exactly similar to how a normally trained model will be deployed.

We will first build a [normal ML pipeline](./Tutorial-Part-2-starting_with_nimbleedge.md) and then convert it into Federated Setting.
