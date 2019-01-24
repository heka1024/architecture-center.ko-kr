---
title: Python 모델의 실시간 점수 매기기
titleSuffix: Azure Reference Architectures
description: 이 참조 아키텍처에서는 Python 모델을 Azure에서 웹 서비스로 배포하여 실시간으로 예측하는 방법을 보여줍니다.
author: msalvaris
ms.date: 11/09/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: 135e86b447684efd9f54340eda4b6bf6e4c35bbb
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54487681"
---
# <a name="real-time-scoring-of-python-scikit-learn-and-deep-learning-models-on-azure"></a><span data-ttu-id="7099b-103">Azure의 Python Scikit-Learn 및 딥러닝 모델의 실시간 채점</span><span class="sxs-lookup"><span data-stu-id="7099b-103">Real-time scoring of Python Scikit-Learn and deep learning models on Azure</span></span>

<span data-ttu-id="7099b-104">이 참조 아키텍처에서는 Python 모델을 웹 서비스로 배포하여 실시간으로 예측하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-104">This reference architecture shows how to deploy Python models as web services to make real-time predictions.</span></span> <span data-ttu-id="7099b-105">두 시나리오에서는 정규 Python 모델을 배포하는 방법 및 딥 러닝 모델을 배포하는 특정 요구 사항을 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-105">Two scenarios are covered: deploying regular Python models, and the specific requirements of deploying deep learning models.</span></span> <span data-ttu-id="7099b-106">시나리오는 모두 표시된 아키텍처를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-106">Both scenarios use the architecture shown.</span></span>

<span data-ttu-id="7099b-107">이 아키텍처의 두 참조 구현은 GitHub에서 지원됩니다([정규 Python 모델][github-python] 및 [딥 러닝 모델][github-dl]에 각각 하나씩).</span><span class="sxs-lookup"><span data-stu-id="7099b-107">Two reference implementations for this architecture are available on GitHub, one for [regular Python models][github-python] and one for [deep learning models][github-dl].</span></span>

![Azure에서 Python 모델의 실시간 채점을 위한 아키텍처 다이어그램](./_images/python-model-architecture.png)

## <a name="scenarios"></a><span data-ttu-id="7099b-109">시나리오</span><span class="sxs-lookup"><span data-stu-id="7099b-109">Scenarios</span></span>

<span data-ttu-id="7099b-110">참조 구현은이 아키텍처를 사용하는 두 가지 시나리오를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-110">The reference implementations demonstrate two scenarios using this architecture.</span></span>

<span data-ttu-id="7099b-111">**시나리오 1: FAQ 일치**.</span><span class="sxs-lookup"><span data-stu-id="7099b-111">**Scenario 1: FAQ matching**.</span></span> <span data-ttu-id="7099b-112">이 시나리오에서는 FAQ(질문과 대답) 일치 모델을 웹 서비스로 배포하여 사용자 질문에 대한 예측을 제공하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-112">This scenario shows how to deploy a frequently asked questions (FAQ) matching model as a web service to provide predictions for user questions.</span></span> <span data-ttu-id="7099b-113">이 시나리오의 경우 아키텍처 다이어그램의 "입력 데이터"는 FAQ 목록과 일치하는 사용자 질문을 포함한 텍스트 문자열을 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-113">For this scenario, "Input Data" in the architecture diagram refers to text strings containing user questions to match with a list of FAQs.</span></span> <span data-ttu-id="7099b-114">이 시나리오는 Python용 [scikit-learn][scikit] 기계 학습 라이브러리에 대해 설계되었지만 Python 모델을 사용하여 실시간으로 예측하는 모든 시나리오에 일반화될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-114">This scenario is designed for the [scikit-learn][scikit] machine learning library for Python, but can be generalized to any scenario that uses Python models to make real-time predictions.</span></span>

<span data-ttu-id="7099b-115">이 시나리오에서는 JavaScript, 해당 중복 질문 및 대답으로 태그가 지정된 원래 질문이 포함된 Stack Overflow 질문 데이터의 하위 집합을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-115">This scenario uses a subset of Stack Overflow question data that includes original questions tagged as JavaScript, their duplicate questions, and their answers.</span></span> <span data-ttu-id="7099b-116">원래 질문 각각과 중복 질문의 일치 확률을 예측하기 위해 scikit-learn 파이프라인을 학습합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-116">It trains a scikit-learn pipeline to predict the match probability of a duplicate question with each of the original questions.</span></span> <span data-ttu-id="7099b-117">REST API 엔드포인트를 사용하여 실시간으로 이러한 예측을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-117">These predictions are made in real time using a REST API endpoint.</span></span>

<span data-ttu-id="7099b-118">이 아키텍처에 대한 애플리케이션 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-118">The application flow for this architecture is as follows:</span></span>

1. <span data-ttu-id="7099b-119">클라이언트는 인코딩된 질문 데이터를 포함한 HTTP POST 요청을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-119">The client sends an HTTP POST request with the encoded question data.</span></span>

2. <span data-ttu-id="7099b-120">Flask 앱은 요청에서 질문을 추출합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-120">The Flask app extracts the question from the request.</span></span>

3. <span data-ttu-id="7099b-121">기능화 및 점수 매기기를 위해 질문을 scikit-learn 파이프라인 모델에 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-121">The question is sent to the scikit-learn pipeline model for featurization and scoring.</span></span>

4. <span data-ttu-id="7099b-122">해당 점수를 포함한 일치 FAQ 질문은 JSON 개체로 파이핑되며 클라이언트에 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-122">The matching FAQ questions with their scores are piped into a JSON object and returned to the client.</span></span>

<span data-ttu-id="7099b-123">결과를 사용하는 예제 앱의 스크린샷은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-123">Here is a screenshot of the example app that consumes the results:</span></span>

![예제 앱 스크린샷](./_images/python-faq-matches.png)

<span data-ttu-id="7099b-125">**시나리오 2: 이미지 분류**.</span><span class="sxs-lookup"><span data-stu-id="7099b-125">**Scenario 2: Image classification**.</span></span> <span data-ttu-id="7099b-126">이 시나리오에서는 CNN(나선형 신경망) 모델을 웹 서비스로 배포하여 이미지에 대한 예측을 제공하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-126">This scenario shows how to deploy a Convolutional Neural Network (CNN) model as a web service to provide predictions on images.</span></span> <span data-ttu-id="7099b-127">이 시나리오의 경우 아키텍처 다이어그램의 "입력 데이터"는 이미지 파일을 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-127">For this scenario, "Input Data" in the architecture diagram refers to image files.</span></span> <span data-ttu-id="7099b-128">CNN은 이미지 분류 및 개체 검색과 같은 작업의 컴퓨터 비전에서 매우 효과적입니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-128">CNNs are very effective in computer vision for tasks such as image classification and object detection.</span></span> <span data-ttu-id="7099b-129">이 시나리오는 TensorFlow, Keras(TensorFlow 백 엔드 포함) 및 PyTorch 프레임워크를 위해 설계되었습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-129">This scenario is designed for the frameworks TensorFlow, Keras (with the TensorFlow back end), and PyTorch.</span></span> <span data-ttu-id="7099b-130">그러나 딥 러닝 모델을 사용하여 실시간으로 예측하는 모든 시나리오에 일반화될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-130">However, it can be generalized to any scenario that uses deep learning models to make real-time predictions.</span></span>

<span data-ttu-id="7099b-131">이 시나리오에서는 ImageNet-1K(1,000개 클래스) 데이터 세트에서 학습된 ResNet-152 미리 학습된 모델을 사용하여 이미지가 속한 범주(아래 그림 참조)를 예측합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-131">This scenario uses a pre-trained ResNet-152 model trained on ImageNet-1K (1,000 classes) dataset to predict which category (see figure below) an image belongs to.</span></span> <span data-ttu-id="7099b-132">REST API 엔드포인트를 사용하여 실시간으로 이러한 예측을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-132">These predictions are made in real time using a REST API endpoint.</span></span>

![예측의 예](./_images/python-example-predictions.png)

<span data-ttu-id="7099b-134">딥 러닝 모델의 애플리케이션 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-134">The application flow for the deep learning model is as follows:</span></span>

1. <span data-ttu-id="7099b-135">클라이언트는 인코딩된 이미지 데이터를 포함한 HTTP POST 요청을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-135">The client sends an HTTP POST request with the encoded image data.</span></span>

2. <span data-ttu-id="7099b-136">Flask 앱은 요청에서 이미지를 추출합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-136">The Flask app extracts the image from the request.</span></span>

3. <span data-ttu-id="7099b-137">이미지는 전처리되고 점수 매기기를 위해 모델로 보내집니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-137">The image is preprocessed and sent to the model for scoring.</span></span>

4. <span data-ttu-id="7099b-138">점수 매기기 결과는 JSON 개체로 파이핑되고 클라이언트에 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-138">The scoring result is piped into a JSON object and returned to the client.</span></span>

## <a name="architecture"></a><span data-ttu-id="7099b-139">아키텍처</span><span class="sxs-lookup"><span data-stu-id="7099b-139">Architecture</span></span>

<span data-ttu-id="7099b-140">이 아키텍처는 다음과 같은 구성 요소로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-140">This architecture consists of the following components.</span></span>

<span data-ttu-id="7099b-141">VM(**[가상 머신][vm]**)</span><span class="sxs-lookup"><span data-stu-id="7099b-141">**[Virtual machine][vm]** (VM).</span></span> <span data-ttu-id="7099b-142">VM은 HTTP 요청을 보낼 수 있는 &mdash; 로컬 또는 &mdash; 클라우드 디바이스의 예제로 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-142">The VM is shown as an example of a device &mdash; local or in the cloud &mdash; that can send an HTTP request.</span></span>

<span data-ttu-id="7099b-143">AKS(**[Azure Kubernetes Service][aks]**)는 Kubernetes 클러스터에서 애플리케이션을 배포하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-143">**[Azure Kubernetes Service][aks]** (AKS) is used to deploy the application on a Kubernetes cluster.</span></span> <span data-ttu-id="7099b-144">AKS는 Kubernetes의 배포 및 작업을 간소화합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-144">AKS simplifies the deployment and operations of Kubernetes.</span></span> <span data-ttu-id="7099b-145">정규 Python 모델의 경우 CPU 전용 VM을 사용하고 딥 러닝 모델의 경우 GPU 설정 VM을 사용하여 클러스터를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-145">The cluster can be configured using CPU-only VMs for regular Python models or GPU-enabled VMs for deep learning models.</span></span>

<span data-ttu-id="7099b-146">**[부하 분산 장치][lb]**</span><span class="sxs-lookup"><span data-stu-id="7099b-146">**[Load balancer][lb]**.</span></span> <span data-ttu-id="7099b-147">AKS에서 프로비전된 부하 분산 장치는 외부에서 서비스를 노출하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-147">A load balancer, provisioned by AKS, is used to expose the service externally.</span></span> <span data-ttu-id="7099b-148">부하 분산 장치의 트래픽은 백 엔드 pod에 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-148">Traffic from the load balancer is directed to the back-end pods.</span></span>

<span data-ttu-id="7099b-149">**[Docker 허브][docker]** 는 Kubernetes 클러스터에 배포된 Docker 이미지를 저장하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-149">**[Docker Hub][docker]** is used to store the Docker image that is deployed on Kubernetes cluster.</span></span> <span data-ttu-id="7099b-150">이 아키텍처에서는 Docker 허브를 사용하기 쉽고 Docker 사용자의 기본 이미지 리포지토리이므로 이 항목을 선택했습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-150">Docker Hub was chosen for this architecture because it's easy to use and is the default image repository for Docker users.</span></span> <span data-ttu-id="7099b-151">이 아키텍처에 [Azure Container Registry][acr]를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-151">[Azure Container Registry][acr] can also be used for this architecture.</span></span>

## <a name="performance-considerations"></a><span data-ttu-id="7099b-152">성능 고려 사항</span><span class="sxs-lookup"><span data-stu-id="7099b-152">Performance considerations</span></span>

<span data-ttu-id="7099b-153">실시간 점수 매기기 아키텍처의 경우 처리량 성능은 중요한 고려 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-153">For real-time scoring architectures, throughput performance becomes a dominant consideration.</span></span> <span data-ttu-id="7099b-154">정규 Python 모델의 경우 일반적으로 워크로드를 처리할 CPU가 충분하다고 여겨집니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-154">For regular Python models, it's generally accepted that CPUs are sufficient to handle the workload.</span></span>

<span data-ttu-id="7099b-155">그러나 딥 러닝 워크로드의 경우 속도가 병목 상태가 되면 GPU는 CPU에 비해 일반적으로 뛰어난 [성능][gpus-vs-cpus]을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-155">However for deep learning workloads, when speed is a bottleneck, GPUs generally provide better [performance][gpus-vs-cpus] compared to CPUs.</span></span> <span data-ttu-id="7099b-156">CPU를 사용하여 GPU 성능을 충족하려면 일반적으로 다수의 CPU를 포함한 클러스터가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-156">To match GPU performance using CPUs, a cluster with large number of CPUs is usually needed.</span></span>

<span data-ttu-id="7099b-157">두 경우 모두 이 아키텍처에 CPU를 사용할 수 있지만 딥 러닝 모델의 경우 GPU는 유사한 비용의 CPU 클러스터에 비해 상당히 높은 처리량 값을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-157">You can use CPUs for this architecture in either scenario, but for deep learning models, GPUs provide significantly higher throughput values compared to a CPU cluster of similar cost.</span></span> <span data-ttu-id="7099b-158">AKS는 GPU를 사용하도록 지원합니다. 이 점은 이 아키텍처에서 AKS를 사용하는 장점 중 하나입니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-158">AKS supports the use of GPUs, which is one advantage of using AKS for this architecture.</span></span> <span data-ttu-id="7099b-159">또한 딥 러닝 배포는 일반적으로 다수의 매개 변수를 포함한 모델을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-159">Also, deep learning deployments typically use models with a high number of parameters.</span></span> <span data-ttu-id="7099b-160">GPU를 사용하면 CPU 전용 배포에서 발생하는 문제인 모델과 웹 서비스 간의 리소스에 대한 경합을 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-160">Using GPUs prevents contention for resources between the model and the web service, which is an issue in CPU-only deployments.</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="7099b-161">확장성 고려 사항</span><span class="sxs-lookup"><span data-stu-id="7099b-161">Scalability considerations</span></span>

<span data-ttu-id="7099b-162">AKS 클러스터가 CPU 전용 VM을 사용하여 프로비전되는 정규 Python 모델의 경우 [pod 수를 확장][manually-scale-pods]할 때 주의하세요.</span><span class="sxs-lookup"><span data-stu-id="7099b-162">For regular Python models, where AKS cluster is provisioned with CPU-only VMs, take care when [scaling out the number of pods][manually-scale-pods].</span></span> <span data-ttu-id="7099b-163">목표는 클러스터를 완벽하게 활용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-163">The goal is to fully utilize the cluster.</span></span> <span data-ttu-id="7099b-164">크기 조정은 pod에 정의된 CPU 요청 및 제한에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-164">Scaling depends on the CPU requests and limits defined for the pods.</span></span> <span data-ttu-id="7099b-165">Kubernetes는 pod의 [자동 크기 조정][autoscale-pods]을 지원하여 CPU 사용률 또는 다른 선택 메트릭에 따라 배포에서 pod 수를 조정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-165">Kubernetes also supports [autoscaling][autoscale-pods] of the pods to adjust the number of pods in a deployment depending on CPU utilization or other select metrics.</span></span> <span data-ttu-id="7099b-166">[클러스터 자동 크기 조정기][autoscaler](미리 보기 상태)는 보류 중인 pod에 따라 에이전트 노드 크기를 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-166">The [cluster autoscaler][autoscaler] (in preview) can scale agent nodes based on pending pods.</span></span>

<span data-ttu-id="7099b-167">GPU 지원 VM을 사용하는 딥 러닝 시나리오의 경우 pod에 대한 리소스 제한은 하나의 pod에 하나의 GPU를 할당하는 것과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-167">For deep learning scenarios, using GPU-enabled VMs, resource limits on pods are such that one GPU is assigned to one pod.</span></span> <span data-ttu-id="7099b-168">사용된 VM의 형식에 따라 [클러스터의 노드를 크기 조정][scale-cluster]하여 서비스에 대한 수요를 충족해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-168">Depending on the type of VM used, you must [scale the nodes of the cluster][scale-cluster] to meet the demand for the service.</span></span> <span data-ttu-id="7099b-169">Azure CLI 및 kubectl을 사용하여 쉽게 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-169">You can do this easily using the Azure CLI and kubectl.</span></span>

## <a name="monitoring-and-logging-considerations"></a><span data-ttu-id="7099b-170">모니터링 및 로깅 고려 사항</span><span class="sxs-lookup"><span data-stu-id="7099b-170">Monitoring and logging considerations</span></span>

### <a name="aks-monitoring"></a><span data-ttu-id="7099b-171">AKS 모니터링</span><span class="sxs-lookup"><span data-stu-id="7099b-171">AKS monitoring</span></span>

<span data-ttu-id="7099b-172">AKS 성능에 대한 가시성은 [컨테이너용 Azure Monitor][monitor-containers] 기능을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-172">For visibility into AKS performance, use the [Azure Monitor for containers][monitor-containers] feature.</span></span> <span data-ttu-id="7099b-173">Metrics API를 통해 Kubernetes에서 사용할 수 있는 컨트롤러, 노드 및 컨테이너의 메모리 및 프로세서 메트릭을 수집합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-173">It collects memory and processor metrics from controllers, nodes, and containers that are available in Kubernetes through the Metrics API.</span></span>

<span data-ttu-id="7099b-174">애플리케이션을 배포하는 동안 AKS 클러스터를 모니터링하여 예상대로 작동하는지, 모든 노드가 작동하는지 및 모든 pods가 실행되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-174">While deploying your application, monitor the AKS cluster to make sure it's working as expected, all the nodes are operational, and all pods are running.</span></span> <span data-ttu-id="7099b-175">[kubectl][kubectl] 명령줄 도구를 사용하여 pod 상태를 검색할 수 있지만 Kubernetes에는 클러스터 상태 및 관리를 기본적으로 모니터링하는 웹 대시보드도 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-175">Although you can use the [kubectl][kubectl] command-line tool to retrieve pod status, Kubernetes also includes a web dashboard for basic monitoring of the cluster status and management.</span></span>

![Kubernetes 대시보드의 스크린샷](./_images/python-kubernetes-dashboard.png)

<span data-ttu-id="7099b-177">클러스터 및 노드의 전반적인 상태를 확인하려면 Kubernetes 대시보드의 **노드** 섹션으로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-177">To see the overall state of the cluster and nodes, go to the **Nodes** section of the Kubernetes dashboard.</span></span> <span data-ttu-id="7099b-178">노드가 비활성화되거나 실패한 경우 해당 페이지의 오류 로그를 표시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-178">If a node is inactive or has failed, you can display the error logs from that page.</span></span> <span data-ttu-id="7099b-179">마찬가지로, pod 수 및 배포의 상태에 대한 정보는 **Pod** 및 **배포** 섹션으로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-179">Similarly, go to the **Pods** and **Deployments** sections for information about the number of pods and status of your deployment.</span></span>

### <a name="aks-logs"></a><span data-ttu-id="7099b-180">AKS 로그</span><span class="sxs-lookup"><span data-stu-id="7099b-180">AKS logs</span></span>

<span data-ttu-id="7099b-181">AKS는 클러스터의 pod 로그에 모든 stdout/stderr을 자동으로 기록합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-181">AKS automatically logs all stdout/stderr to the logs of the pods in the cluster.</span></span> <span data-ttu-id="7099b-182">kubectl을 사용하여 이러한 기록 및 노드 수준 이벤트 및 로그를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-182">Use kubectl to see these and also node-level events and logs.</span></span> <span data-ttu-id="7099b-183">자세한 내용은 배포 단계를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7099b-183">For details, see the deployment steps.</span></span>

<span data-ttu-id="7099b-184">[컨테이너용 Azure Monitor][monitor-containers]를 사용하여 Linux용 Log Analytics 에이전트의 컨테이너화된 버전을 통해 메트릭 및 로그를 수집합니다. 이 내용은 Log analytics 작업 영역에 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-184">Use [Azure Monitor for containers][monitor-containers] to collect metrics and logs through a containerized version of the Log Analytics agent for Linux, which is stored in your Log Analytics workspace.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="7099b-185">보안 고려 사항</span><span class="sxs-lookup"><span data-stu-id="7099b-185">Security considerations</span></span>

<span data-ttu-id="7099b-186">[Azure Security Center][security-center]를 사용하여 Azure 리소스의 보안 상태를 중앙에서 살펴볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-186">Use [Azure Security Center][security-center] to get a central view of the security state of your Azure resources.</span></span> <span data-ttu-id="7099b-187">Security Center는 잠재적인 보안 문제를 모니터링하고 배포의 보안 상태에 대한 종합적인 그림을 제공합니다. 하지만 AKS 에이전트 노드는 모니터링하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-187">Security Center monitors potential security issues and provides a comprehensive picture of the security health of your deployment, although it doesn't monitor AKS agent nodes.</span></span> <span data-ttu-id="7099b-188">보안 센터는 각 Azure 구독을 기준으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-188">Security Center is configured per Azure subscription.</span></span> <span data-ttu-id="7099b-189">[Security Center 표준에 Azure 구독 온보딩][get-started]에서 설명된 대로 보안 데이터 컬렉션을 활성화합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-189">Enable security data collection as described in [Onboard your Azure subscription to Security Center Standard][get-started].</span></span> <span data-ttu-id="7099b-190">데이터 수집이 사용되도록 설정되면 보안 센터는 해당 구독에서 만든 모든 VM을 자동으로 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-190">When data collection is enabled, Security Center automatically scans any VMs created under that subscription.</span></span>

<span data-ttu-id="7099b-191">**작업**</span><span class="sxs-lookup"><span data-stu-id="7099b-191">**Operations**.</span></span> <span data-ttu-id="7099b-192">Azure AD(Azure Active Directory) 인증 토큰을 사용하여 AKS 클러스터에 로그인하려면 [사용자 인증][aad-auth]에 대해 Azure AD를 사용하도록 AKS를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-192">To sign in to an AKS cluster using your Azure Active Directory (Azure AD) authentication token, configure AKS to use Azure AD for [user authentication][aad-auth].</span></span> <span data-ttu-id="7099b-193">클러스터 관리자는 사용자 ID 또는 디렉터리 그룹 멤버 자격에 따라 Kubernetes RBAC(역할 기반 액세스 제어)를 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-193">Cluster administrators can also configure Kubernetes role-based access control (RBAC) based on a user's identity or directory group membership.</span></span>

<span data-ttu-id="7099b-194">[RBAC][rbac]를 사용하여 배포하는 Azure 리소스에 대한 액세스를 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-194">Use [RBAC][rbac] to control access to the Azure resources that you deploy.</span></span> <span data-ttu-id="7099b-195">RBAC를 통해 DevOps 팀의 구성원에게 권한 역할을 할당할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-195">RBAC lets you assign authorization roles to members of your DevOps team.</span></span> <span data-ttu-id="7099b-196">한 명의 사용자가 여러 역할에 할당될 수 있으며 좀 더 세분화된 [사용 권한]의 사용자 지정 역할을 만들 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-196">A user can be assigned to multiple roles, and you can create custom roles for even more fine-grained [permissions].</span></span>

<span data-ttu-id="7099b-197">**HTTPS**</span><span class="sxs-lookup"><span data-stu-id="7099b-197">**HTTPS**.</span></span> <span data-ttu-id="7099b-198">보안을 극대화하려면 애플리케이션에서는 HTTPS를 적용하고 HTTP 요청을 리디렉션해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-198">As a security best practice, the application should enforce HTTPS and redirect HTTP requests.</span></span> <span data-ttu-id="7099b-199">[수신 컨트롤러][ingress-controller]를 사용하여 SSL을 종료하고 HTTP 요청을 리디렉션하는 역방향 프록시를 배포합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-199">Use an [ingress controller][ingress-controller] to deploy a reverse proxy that terminates SSL and redirects HTTP requests.</span></span> <span data-ttu-id="7099b-200">자세한 내용은 [AKS(Azure Kubernetes Service)에 HTTPS 수신 컨트롤러 만들기][https-ingress]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7099b-200">For more information, see [Create an HTTPS ingress controller on Azure Kubernetes Service (AKS)][https-ingress].</span></span>

<span data-ttu-id="7099b-201">**인증**.</span><span class="sxs-lookup"><span data-stu-id="7099b-201">**Authentication**.</span></span> <span data-ttu-id="7099b-202">이 솔루션은 엔드포인트에 대한 액세스를 제한하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-202">This solution doesn't restrict access to the endpoints.</span></span> <span data-ttu-id="7099b-203">엔터프라이즈 환경에서 아키텍처를 배포하려면 API 키를 통해 엔드포인트를 보호하고, 클라이언트 애플리케이션에 특정 형태의 사용자 인증을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-203">To deploy the architecture in an enterprise setting, secure the endpoints through API keys and add some form of user authentication to the client application.</span></span>

<span data-ttu-id="7099b-204">**컨테이너 레지스트리**</span><span class="sxs-lookup"><span data-stu-id="7099b-204">**Container registry**.</span></span> <span data-ttu-id="7099b-205">이 솔루션에서는 Docker 이미지를 저장하는 데 공개 레지스트리를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-205">This solution uses a public registry to store the Docker image.</span></span> <span data-ttu-id="7099b-206">애플리케이션에서 사용하는 코드 및 모델은 이 이미지에 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-206">The code that the application depends on, and the model, are contained within this image.</span></span> <span data-ttu-id="7099b-207">엔터프라이즈 애플리케이션은 비공개 레지스트리를 사용하여 악의적인 코드 실행으로부터 보호하고 컨테이너 내의 정보를 손상되지 않도록 유지해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-207">Enterprise applications should use a private registry to help guard against running malicious code and to help keep the information inside the container from being compromised.</span></span>

<span data-ttu-id="7099b-208">**DDoS 보호**</span><span class="sxs-lookup"><span data-stu-id="7099b-208">**DDoS protection**.</span></span> <span data-ttu-id="7099b-209">[DDoS Protection 표준][ddos]을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-209">Consider enabling [DDoS Protection Standard][ddos].</span></span> <span data-ttu-id="7099b-210">기본 DDoS 보호가 Azure 플랫폼의 일부로 활성화되어 있지만 DDoS Protection 표준은 Azure Virtual Network 리소스에 맞게 조정된 완화 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-210">Although basic DDoS protection is enabled as part of the Azure platform, DDoS Protection Standard provides mitigation capabilities that are tuned specifically to Azure virtual network resources.</span></span>

<span data-ttu-id="7099b-211">**로깅**</span><span class="sxs-lookup"><span data-stu-id="7099b-211">**Logging**.</span></span> <span data-ttu-id="7099b-212">보안 사기에 사용될 수 있는 사용자 암호 및 기타 정보를 삭제하는 등 로그 데이터를 저장하기 전에 모범 사례를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7099b-212">Use best practices before storing log data, such as scrubbing user passwords and other information that could be used to commit security fraud.</span></span>

## <a name="deployment"></a><span data-ttu-id="7099b-213">배포</span><span class="sxs-lookup"><span data-stu-id="7099b-213">Deployment</span></span>

<span data-ttu-id="7099b-214">이 참조 아키텍처를 배포하려면 GitHub 리포지토리에 설명된 단계를 따르세요.</span><span class="sxs-lookup"><span data-stu-id="7099b-214">To deploy this reference architecture, follow the steps described in the GitHub repos:</span></span>

- <span data-ttu-id="7099b-215">[정규 Python 모델][github-python]</span><span class="sxs-lookup"><span data-stu-id="7099b-215">[Regular Python models][github-python]</span></span>
- <span data-ttu-id="7099b-216">[딥 러닝 모델][github-dl]</span><span class="sxs-lookup"><span data-stu-id="7099b-216">[Deep learning models][github-dl]</span></span>

<!-- links -->

[aad-auth]: /azure/aks/aad-integration
[acr]: /azure/container-registry/
[something]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/
[aks]: /azure/aks/intro-kubernetes
[autoscaler]: /azure/aks/autoscaler
[autoscale-pods]: /azure/aks/tutorial-kubernetes-scale#autoscale-pods
[azcopy]: /azure/storage/common/storage-use-azcopy-linux
[ddos]: /azure/virtual-network/ddos-protection-overview
[docker]: https://hub.docker.com/
[get-started]: /azure/security-center/security-center-get-started
[github-python]: https://github.com/Azure/MLAKSDeployment
[github-dl]: https://github.com/Microsoft/AKSDeploymentTutorial
[gpus-vs-cpus]: https://azure.microsoft.com/en-us/blog/gpus-vs-cpus-for-deployment-of-deep-learning-models/
[https-ingress]: /azure/aks/ingress-tls
[ingress-controller]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[kubectl]: https://kubernetes.io/docs/tasks/tools/install-kubectl/
[lb]: /azure/load-balancer/load-balancer-overview
[manually-scale-pods]: /azure/aks/tutorial-kubernetes-scale#manually-scale-pods
[monitor-containers]: /azure/monitoring/monitoring-container-insights-overview
[사용 권한]: /azure/aks/concepts-identity
[permissions]: /azure/aks/concepts-identity
[rbac]: /azure/active-directory/role-based-access-control-what-is
[scale-cluster]: /azure/aks/scale-cluster
[scikit]: https://pypi.org/project/scikit-learn/
[security-center]: /azure/security-center/security-center-intro
[vm]: /azure/virtual-machines/
