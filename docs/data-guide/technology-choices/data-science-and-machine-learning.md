---
title: Machine Learning 기술 선택
description: 기계 학습 모델을 빌드, 배포 및 관리하는 옵션을 비교합니다. 솔루션에 대해 어떤 Microsoft 제품을 선택할지 결정합니다.
author: MikeWasson
ms.date: 03/06/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: e4c81add1a97254f427d67584ff7e2a4799f84a9
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640909"
---
# <a name="what-are-the-machine-learning-products-at-microsoft"></a>Microsoft의 기계 학습 제품이란?

머신 러닝은 컴퓨터에서 기존 데이터를 사용하여 미래 동작, 결과 및 추세를 예측하는 데이터 과학 기술입니다. 머신 러닝을 사용하면 컴퓨터에서 명시적으로 프로그래밍하지 않고 학습합니다.

기계 학습 솔루션을 반복적으로 작성 된 있고 단계로:

- 데이터 준비
- 실험 및 모델 학습
- 학습 된 모델 배포
- 모델 배포 관리

Microsoft는 다양 한 제품 준비, 작성, 배포 및 기계 학습 모델을 관리 하는 옵션을 제공 합니다. 이러한 제품을 비교하고 기계 학습 솔루션을 가장 효과적으로 개발해야 하는 항목을 선택합니다.

## <a name="cloud-based-options"></a>클라우드 기반 옵션

다음 옵션은 Azure 클라우드에서 기계 학습에 사용할 수 있습니다.

| 클라우드&nbsp;옵션 | 정의 | 수행할 수 있는 작업 |
|-|-|-|
| [Azure Machine Learning 서비스](#azure-machine-learning-service) | Machine learning 위한 관리 되는 클라우드 서비스  | Python 및 CLI를 사용하여 Azure에서 모델을 학습, 배포 및 관리 |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | 끌기&ndash;고&ndash;machine learning 위한 놓기 시각적 인터페이스 | 미리 구성된 알고리즘을 사용하여 모델을 빌드, 실험 및 배포 |

미리 작성 된 AI 및 기계 학습 모델을 사용 하려는 경우 [Azure Cognitive Services](#azure-cognitive-services) 쉽게 응용 프로그램에 지능형 기능을 추가할 수 있습니다.

## <a name="on-premises-options"></a>온-프레미스 옵션

다음 옵션은 기계 학습 온-프레미스에 사용할 수 있습니다. 온-프레미스 서버도 클라우드의 가상 머신에서 실행할 수 있습니다.

| 온-프레미스&nbsp;옵션 | 정의 | 수행할 수 있는 작업 |
|-|-|-|
| [SQL Server Machine Learning 서비스](#sql-server-machine-learning-services) | SQL에 포함된 Analytics 엔진 | SQL Server 내에 모델을 빌드 및 배포 |
| [Microsoft Machine Learning Server](#microsoft-machine-learning-server) | 예측 분석에 대한 독립 실행형 엔터프라이즈 서버 | 전처리 된 데이터에서 모델 빌드 및 배포 |

## <a name="development-platforms-and-tools"></a>개발 플랫폼 및 도구

다음과 같은 개발 플랫폼 및 도구를 machine learning을 위해 사용할 수 있습니다.

| 플랫폼/도구 | 정의 | 수행할 수 있는 작업 |
|-|-|-|
| [Azure Data Science Virtual Machine](#azure-data-science-virtual-machine) | 미리 설치된 데이터 과학 도구를 사용한 가상 머신 | 미리 구성 된 환경에서 기계 학습 솔루션 개발 |
| [Azure Databricks](#azure-databricks) | Spark 기반 분석 플랫폼 | 모델 및 데이터 워크플로를 빌드 및 배포 |
| [ML.NET](#mlnet) | SDK를 학습 하는 오픈 소스, 플랫폼 간 컴퓨터 | .NET 응용 프로그램에 대 한 기계 학습 솔루션 개발 |
| [Windows ML](#windows-ml) | Windows 10 컴퓨터 학습 플랫폼입니다. | Windows 10 디바이스에서 학습된 모델을 평가 |

## <a name="azure-machine-learning-service"></a>Azure Machine Learning 서비스

[Azure Machine Learning 서비스](/azure/machine-learning/service/overview-what-is-azure-ml.md) 는 학습, 배포 및 규모의 기계 학습 모델을 관리 하는 데 사용 하는 완전 관리형된 클라우드 서비스입니다. 이 서비스는 오픈 소스 기술을 완벽히 지원하여 TensorFlow, PyTorch 및 scikit-learn 등의 수많은 오픈 소스 Python 패키지를 사용할 수 있습니다. [Azure 노트북](https://notebooks.azure.com/), [Jupyter 노트북](http://jupyter.org) 또는 [Visual Studio Code용 Azure Machine Learning](https://aka.ms/vscodetoolsforai)와 같이 다양한 도구도 제공되어 데이터를 쉽게 검색하고 변환하게 한 다음, 모델을 학습 및 배포합니다. Azure Machine Learning 서비스에는 간편하고 효율적이고 정확한 모델 생성 및 튜닝을 자동화하는 기능이 포함됩니다.

학습, 배포 및 Python 및 CLI를 사용 하 여 클라우드 규모의 기계 학습 모델을 관리 하려면 Azure Machine Learning 서비스를 사용 합니다.

[Azure Machine Learning 서비스의 평가판 또는 유료 버전](https://aka.ms/AMLFree)을 사용해 보세요.

|||
|-|-|
|**형식**                   |클라우드 기반 기계 학습 솔루션|
|**지원되는 언어**    |Python|
|**Machine learning 단계**|데이터 준비<br>모델 학습<br>배포<br>관리|
|**주요 이점**           |스크립트 및 실행 기록이 중앙에서 관리되므로 모델 버전을 쉽게 비교할 수 있습니다.<br/><br/>클라우드 또는 에지 디바이스로 모델을 쉽게 배포 및 관리합니다.|
|**고려 사항**         |모델 관리 모델에 어느 정도 숙지해야 합니다.|

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

[Azure Machine Learning Studio](/azure/machine-learning/studio/)는 대화형 시각적 작업 영역을 제공하여 미리 빌드된 기계 학습 알고리즘을 통해 모델을 쉽고 빠르게 빌드, 테스트 및 배포하는 데 사용할 수 있습니다. Machine Learning Studio는 Excel과 같은 BI 도구 또는 사용자 지정 앱에서 쉽게 사용할 수 있는 웹 서비스로 모델을 게시합니다.
프로그래밍이 필요없습니다 - 대화형 캔버스에서 데이터 집합 및 분석 모듈을 연결하여 기계 학습 모델을 구성한 다음, 몇 번의 클릭으로 배포합니다.

코드가 필요없는 모델을 개발하고 배포하려는 경우 Machine Learning Studio를 사용하세요.

[Azure Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2&target=_blank)를 유료 또는 무료로 사용해 보세요.

|||
|-|-|
|**형식**                   |끌어서 놓기, 클라우드 기반 기계 학습 솔루션|
|**지원되는 언어**    |Python, R|
|**Machine learning 단계**|데이터 준비<br>모델 학습<br>배포<br>관리|
|**주요 이점**           |대화형 시각적 인터페이스를 사용하면 최소한의 코드로 Machine Learning 모델링을 수행할 수 있습니다.<br/><br/>데이터 탐색을 위한 기본 제공 Jupyter 노트<br/><br/>학습된 모델을 Azure 웹 서비스로서 직접 배포|
|**고려 사항**         |제한된 확장성. 학습 데이터 세트의 최대 크기는 10GB입니다.<br/><br/>온라인 전용 오프라인 개발 환경은 없습니다.|

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

[Azure Cognitive Services](/azure/cognitive-services/welcome)는 자연스러운 통신 방법을 사용하는 앱을 빌드할 수 있는 API 집합입니다. 이러한 API를 사용하면 단 몇 줄의 코드로 사용자 앱이 사용자의 요구를 보고, 듣고, 말하고, 이해하며, 해석할 수 있습니다. 다음과 같은 앱에 지능형 기능을 쉽게 추가합니다.

- 이모티콘 및 감정 검색
- 음성 인식
- LUIS(언어 해석)
- 지식 및 검색

Cognitive Services를 사용하여 장치 및 플랫폼에서 앱을 개발합니다. API는 끊임없이 개선되며, 설치하기가 매우 쉽습니다.

|||
|-|-|
|**형식**                   |지능형 응용 프로그램을 작성 하기 위한 Api|
|**지원되는 언어**    |서비스에 따라 다양 한 옵션|
|**Machine learning 단계**|배포|
|**주요 이점**           |미리 학습 된 모델을 사용 하 여 응용 프로그램에서 기계 학습 기능을 통합 합니다.<br/><br/>다양 한 시각 및 음성을 사용 하 여 자연 스러운 통신 방법에 대 한 모델입니다.|
|**고려 사항**         |미리 학습 된 모델과 사용자 지정할 수 없습니다.|

## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning 서비스

[SQL Server Microsoft Machine Learning 서비스](https://docs.microsoft.com/sql/advanced-analytics/r/r-services)는 SQL Server 데이터베이스의 관계형 데이터를 위해 R 및 Python에 통계 분석, 데이터 시각화 및 예측 분석을 추가합니다. SQL Server의 고급 모델링 및 기계 학습 알고리즘을 대규모 및 병렬로 실행 수를 포함 하는 Microsoft에서 R 및 Python 라이브러리입니다.

기본 제공 AI 및 SQL Server의 관계형 데이터에 대한 예측 분석이 필요한 경우 SQL Server Machine Learning Services를 사용합니다.

|||
|-|-|
|**형식**                   |온-프레미스 관계형 데이터에 대 한 예측 분석|
|**지원되는 언어**    |Python, R|
|**Machine learning 단계**|데이터 준비<br>모델 학습<br>배포|
|**주요 이점**           |예측 논리를 데이터베이스 함수에 캡슐화하여 데이터 계층 논리에 쉽게 포함할 수 있습니다.|
|**고려 사항**         |SQL Server Database를 애플리케이션에 대한 데이터 계층으로 간주합니다.|

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning 서버

[Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)는 R 및 Python 프로세스의 병렬 및 분산 워크로드를 호스트하고 관리하기 위한 엔터프라이즈 서버입니다. Microsoft Machine Learning Server는 Linux, Windows, Hadoop 및 Apache Spark에서 실행되며, [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/)에서도 사용할 수 있습니다. 이 서버는 [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) 및 [MicrosoftML 패키지](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)를 사용하여 빌드된 솔루션용 실행 엔진을 제공하며, 고성능 분석, 통계 분석, 기계 학습 및 대규모 데이터 세트를 지원하여 오픈 소스 R 및 Python을 확장합니다. 이 기능은 서버와 함께 설치되는 전용 패키지를 통해 제공됩니다. 개발을 위해 [Visual Studio용 R 도구](https://www.visualstudio.com/vs/rtvs/) 및 [Visual Studio용 Python 도구](https://www.visualstudio.com/vs/python/)와 같은 IDE를 사용할 수 있습니다.

서버에서 R 및 Python을 통해 구축된 모델을 빌드하고 운영하거나, Hadoop 또는 Spark 클러스터에서 대규모로 학습하는 R 및 Python을 배포해야 하는 경우 Microsoft Machine Learning Server를 사용합니다.

|||
|-|-|
|**형식**                   |예측 분석을 위해 온-프레미스 엔터프라이즈 서버|
|**지원되는 언어**    |Python, R|
|**Machine learning 단계**|모델 학습<br>배포|
|**주요 이점**           |높은 확장성.|
|**고려 사항**         |엔터프라이즈에서 Machine Learning Server를 배포하고 관리해야 합니다.|

## <a name="azure-data-science-virtual-machine"></a>Azure 데이터 과학 가상 머신

[Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview)은 데이터 과학을 수행하기 위해 특별히 빌드된 Microsoft Azure 클라우드에서 사용자 지정된 가상 머신 환경입니다. 여기에는 고급 분석을 위한 지능형 응용 프로그램 구축에 바로 뛰어들 수 있도록 다수의 유명한 데이터 과학 및 기타 도구가 미리 설치 및 구성되어 있습니다.

Data Science Virtual Machine은 Azure Machine Learning 서비스의 대상으로 지원됩니다.
또한 Windows와 Linux Ubuntu용 버전에서도 사용할 수 있습니다(Linux CentOS에서는 Azure Machine Learning 서비스를 지원하지 않습니다).
특정 버전 정보 및 포함된 항목 목록은 [Azure Data Science Virtual Machine 소개](/azure/machine-learning/data-science-virtual-machine/overview.md)를 참조하세요.

단일 노드에서 작업을 실행하거나 호스트해야 하는 경우 데이터 과학 VM을 사용하세요. 또는 단일 컴퓨터에서 처리를 원격으로 강화해야 하는 경우에 사용합니다.

|||
|-|-|
|**형식**                   |데이터 과학을 위한 사용자 지정된 가상 머신 환경|
|**주요 이점**           |데이터 과학 도구 및 프레임워크를 설치 및 관리하고 문제를 해결하는 데 드는 시간을 단축할 수 있습니다.<br/><br/>가장 일반적으로 사용되는 도구 및 프레임워크의 최신 버전이 포함되어 있습니다.<br/><br/>가상 머신 옵션에는 집약적 데이터 모델링을 위한 GPU 기능이 있는 확장성 높은 이미지가 포함되어 있습니다.|
|**고려 사항**         |오프라인 상태에서는 가상 머신에 액세스할 수 없습니다.<br/><br/>가상 머신을 실행하면 Azure 요금이 발생하므로, 필요할 때만 실행하도록 주의해야 합니다.|

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)는 Microsoft Azure Cloud Services 플랫폼에 대해 최적화된 Apache Spark 기반 분석 플랫폼입니다. Databricks는 Azure와 통합되어 데이터 과학자, 데이터 엔지니어, 비즈니스 분석가가 공동 작업할 수 있도록 하는 대화형 작업 영역, 간소화된 워크플로 및 원클릭 설정을 제공합니다.
웹 기반 노트북에서 Python, R, Scala 및 SQL 코드를 사용하여 데이터를 쿼리, 시각화 및 모델링합니다.

Apache Spark에서 기계 학습 솔루션을 빌드하는 데 공동 작업하려는 경우 Databricks를 사용합니다.

|||
|-|-|
|**형식**                   |Apache Spark 기반 분석 플랫폼|
|**지원되는 언어**    |Python, R, Scala, SQL|
|**Machine learning 단계**|데이터 쿼리<br>모델 학습|

## <a name="mlnet"></a>ML.NET

[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/)는 무료, 오픈 소스, 플랫폼 간 기계 학습 프레임워크로서 사용자 지정 기계 학습 솔루션을 빌드해서 .NET 애플리케이션에 통합할 수 있습니다.

.NET 애플리케이션에 기계 학습 솔루션을 통합하려는 경우 ML.NET를 사용합니다.

|||
|-|-|
|**형식**                   |사용자 지정 기계 학습 응용 프로그램을 개발 하기 위한 오픈 소스 프레임 워크|
|**지원 되는 언어**    |.NET|

## <a name="windows-ml"></a>Windows ML

[Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/) 유추 엔진을 사용 하면 학습 된 기계 학습 응용 프로그램에서 모델, Windows 10 장치에서 로컬로 학습 된 모델 평가 사용할 수 있습니다.

Windows 애플리케이션 내에서 학습된 기계 학습 모델을 사용하려는 경우 Windows ML을 사용합니다.

|||
|-|-|
|**형식**                   |Windows 장치에 학습 된 모델에 대 한 유추 엔진|
|**지원 되는 언어**    |C#/C++, JavaScript|

## <a name="next-steps"></a>다음 단계

- Microsoft에서 사용 가능한 모든 AI (인공 지능) 개발 제품을 알아보려면 [Microsoft AI 플랫폼](https://www.microsoft.com/ai)
- AI 솔루션을 개발하는 방법에 대한 학습은 [Microsoft AI School](https://aischool.microsoft.com/learning-paths)을 참조
