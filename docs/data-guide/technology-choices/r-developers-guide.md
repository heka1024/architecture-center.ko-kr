---
title: Azure R 개발자 가이드 - R 프로그래밍 | Microsoft Docs
description: 이 문서에서는 데이터 과학자가 Azure에서 R 프로그래밍 언어와 기존 기술을 활용하는 다양한 방법을 간략히 보여줍니다. Azure는 R 개발자가 데이터 과학 워크 로드를 클라우드로 확장 하는 데 사용할 수 있는 많은 서비스를 제공 합니다.
services: machine-learning
author: AnalyticJeremy
ms.service: machine-learning
ms.workload: data-services
ms.devlang: R
ms.topic: article
ms.date: 03/19/2019
ms.author: jepeach
ms.openlocfilehash: a14c8ce76f78baa7274f22b939eb28cb025ef87e
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58888116"
---
# <a name="r-developers-guide-to-azure"></a>Azure에 대한 R 개발자 가이드

<img src="../images/logo_r.svg" alt="R logo" align="right" width="200" />

끊임없이 늘어나는 데이터를 다루는 여러 데이터 과학자는 클라우드 컴퓨팅의 강점을 분석에 활용할 수 있는 방법을 찾고 있습니다.  이 문서에서는 데이터 과학자가 Azure에서 [R 프로그래밍 언어](https://www.r-project.org)와 기존 기술을 활용하는 다양한 방법을 간략히 보여줍니다.

Microsoft은 데이터 과학자에게 최고의 도구인 R 프로그래밍 언어를 로 완벽하게 도입했습니다.  회사는 R 개발자가 Azure에서 코드를 실행할 때 사용할 수 있는 다양한 옵션을 제공하여 데이터 과학자가 대규모 프로젝트 진행 시 데이터 과학 워크로드를 클라우드로 확장할 수 있게 해줍니다.

다양한 옵션 및 각 옵션에 가장 적합한 시나리오를 살펴보겠습니다.

## <a name="azure-services-with-r-language-support"></a>R 언어를 지원하는 Azure 서비스

이 문서에서는 R 언어를 지원하는 다음과 같은 Azure 서비스를 다룹니다.

|서비스                                                          |설명                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
|[데이터 과학 Virtual Machine](#data-science-virtual-machine)    |데이터 과학 워크스테이션 또는 사용자 지정 계산 대상으로 사용할 사용자 지정된 VM|
|[HDInsight의 ML 서비스](#ml-services-on-hdinsight)            |여러 노드 간에 대규모 데이터 세트에 대한 R 분석을 실행하기 위한 클러스터 기반 시스템   |
|[Azure Databricks](#azure-databricks)                            |R 및 다른 언어를 지원하는 공동 작업 Spark 환경               |
|[Azure Machine Learning Studio](#azure-machine-learning-studio)  |Azure의 기계 학습 실험에서 사용자 지정 R 스크립트 실행                      |
|[Azure Batch](#azure-batch)                                      |클러스터의 여러 노드 간에 R 코드를 경제적으로 실행하기 위한 다양한 옵션 제공|
|[Azure Notebooks](#azure-notebooks)                              |무료 클라우드 기반 버전의 Jupyter 노트북                  |
|[Azure SQL Database](#azure-sql-database)                        |SQL Server 데이터베이스 엔진 내에서 R 스크립트 실행                            |

## <a name="data-science-virtual-machine"></a>데이터 과학 Virtual Machine

[DSVM(데이터 과학 Virtual Machine)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview)은 데이터 과학 수행을 위해 특별히 구축된 Microsoft Azure 클라우드 플랫폼의 사용자 지정 VM 이미지입니다. 다음을 비롯한 여러 주요 데이터 과학 도구가 있습니다.

* [Microsoft R Open](https://mran.microsoft.com/open/)
* [Microsoft Machine Learning 서버](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop)
* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server)

Windows 또는 Linux에서 DSVM을 운영 체제로 프로비전할 수 있습니다.  DSVM은 대화형 워크스테이션 또는 사용자 지정 클러스터의 계산 플랫폼으로 사용할 수 있습니다.

### <a name="as-a-workstation"></a>워크스테이션으로 사용

클라우드에서 쉽고 빠르게 R을 시작하려면 이 방법을 사용하는 것이 가장 좋습니다.  이 환경은 로컬 워크스테이션에서 R로 작업해본 경험이 있다면 익숙할 것입니다.  하지만 R 환경은 로컬 리소스를 사용하는 대신 클라우드의 VM에서 실행됩니다.  Azure에 데이터가 이미 저장된 경우 R 스크립트가 "데이터에 더 가깝게" 실행되도록 할 수 있다는 추가적인 장점이 있습니다. 인터넷을 통해 데이터를 전송하는 대신 더 빠르게 액세스할 수 있는 Azure의 내부 네트워크를 통해 데이터에 액세스할 수 있습니다.

DSVM은 소규모 R 개발자 팀에게 특히 유용할 수 있습니다.  각 개발자는 각 개발자의 강력한 워크스테이션에 투자하고, 팀원에게 동일한 버전의 소프트웨어 패키지를 사용하도록 요구하는 대신 필요할 때마다 DSVM 인스턴스를 구동할 수 있습니다.

### <a name="as-a-compute-platform"></a>계산 플랫폼으로 사용

DSVM은 워크스테이션으로 사용할 수 있을 뿐만 아니라 R 프로젝트에 맞게 유연하게 확장할 수 있는 계산 플랫폼으로도 사용할 수 있습니다.  사용 하는 [ `AzureDSVM` ](https://github.com/Azure/AzureDSVM) R 패키지를 생성 및 삭제 DSVM 인스턴스를 프로그래밍 방식으로 제어할 수 있습니다.  인스턴스를 클러스터로 형성하고 클라우드에서 수행할 분산 분석을 배포할 수 있습니다.  로컬 워크스테이션에서 실행되는 R 코드로 이 전체 프로세스를 제어할 수 있습니다.

DSVM에 대 한 자세한 내용은 참조 하세요 [Linux 및 Windows 용 Azure 데이터 과학 Virtual Machine에 소개](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview)합니다.

## <a name="ml-services-on-hdinsight"></a>HDInsight의 ML 서비스

[Microsoft ML Services](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview)는 데이터 과학자, 통계학자 및 R 프로그래머에게 HDInsight의 확장 가능한 분산형 분석 방법에 요청 시 액세스할 수 있도록 해줍니다.  이 솔루션은 Azure Blob 또는 Data Lake Store에 로드된 거의 모든 크기의 데이터 세트에서 R 기반 분석을 위한 최신 기능을 제공합니다.

이는 클러스터 간에 R 코드를 확장할 수 있게 해주는 엔터프라이즈급 솔루션입니다.  Microsoft의 기능을 활용 하 여 [`RevoScaleR`](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)
패키지를 HDInsight에서 R 스크립트 데이터 처리 기능 동시에 실행할 수는 클러스터의 여러 노드에서 합니다.  그러면 R이 워크스테이션에서 실행 중인 단일 스레드 R보다 훨씬 더 큰 규모의 데이터를 고속으로 처리할 수 있습니다.

이러한 확장 기능을 제공하는 HDInsight의 ML Services는 대량의 데이터 집합을 다루는 R 개발자에게 유용한 옵션입니다.  클라우드에서 R 스크립트를 실행하는 데 필요한 유연하고 확장 가능한 플랫폼을 제공합니다.

ML 서비스 클러스터를 만드는 연습을 참조 하세요 [Azure HDInsight에서 기계 학습 서비스를 사용 하 여 시작](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started)합니다.

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](https://azure.microsoft.com/services/databricks/)는 Microsoft Azure Cloud Services 플랫폼에 대해 최적화된 Apache Spark 기반 분석 플랫폼입니다.  Apache Spark의 기반으로 설계된 Databricks는 Azure와 통합되어 원클릭 설정, 간소화된 워크플로 및 데이터 과학자, 데이터 엔지니어, 비즈니스 분석가가 공동 작업할 수 있도록 하는 대화형 작업 영역을 제공합니다.

Databricks의 공동 작업은 플랫폼 노트북 시스템에서 지원합니다.  사용자는 다른 시스템 사용자와 함께 노트북을 만들고, 공유하고, 편집할 수 있습니다.  사용자는 이러한 노트북을 통해 Databricks 환경에서 관리되는 Spark 클러스터에 실행되는 코드를 작성할 수 있습니다.  이러한 노트북은 R을 완벽하게 지원하며, `SparkR` 및 `sparklyr` 패키지 모두를 통해 사용자에게 Spark에 대한 액세스를 제공합니다.

Databricks는 Spark에 기반을 두고 있고, 공동 작업에 특히 중점을 두고 있으므로 대규모 데이터 집합의 복잡한 분석을 위해 협력하는 데이터 과학자 팀이 사용하는 경우가 많습니다.  Databricks의 노트북은 R 외에 다른 언어도 지원하므로 분석가가 주 작업에 여러 언어를 사용하는 팀에 특히 유용합니다.

이 문서 [Azure Databricks 란?](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) 플랫폼 및 시작할 수 있도록 지원 하는 방법에 대 한 자세한 세부 정보를 제공할 수 있습니다.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

[Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/)는 클라우드에서 예측 분석 솔루션을 빌드, 테스트 및 배포하는 데 사용할 수 있는 공동 끌어서 놓기 도구입니다.  신흥 데이터 과학자가 대량의 코드를 작성할 필요 없이 기계 학습 모델을 만들고 배포하도록 지원합니다.

Azure Machine Learning Studio에는 R 및 Python 모두 지원합니다.  두 가지 방법으로 Azure Machine Learning Studio를 사용 하 여 R을 사용할 수 있습니다.

### <a name="custom-r-scripts-in-your-experiments"></a>실험의 사용자 지정 R 스크립트

먼저 사용자 지정 R 스크립트를 작성하여 ML Studio의 데이터 조작 및 기계 학습 기능을 확장할 수 있습니다.
ML Studio에는 데이터 준비 및 분석을 위한 다양한 모듈이 포함되어 있지만 R 같은 성숙한 언어의 기능과 매칭할 수는 없습니다. 따라서 이 서비스는 제공된 모듈이 요구 사항에 맞지 않을 경우 사용자 지정 R 스크립트를 도입할 수 있게 설계되었습니다.

이 기능을 활용하려면 "R 스크립트 실행" 모듈을 실험에 끌어다 놓습니다.  그런 다음, "속성" 창의 코드 편집기를 사용하여 새로운 R 스크립트를 작성하거나 기존의 스크립트를 붙여넣습니다.  스크립트 내에서 외부 R 패키지를 참조할 수 있습니다.  표준 Azure Machine Learning Studio 모델 라이브러리의 일부분이 아닌 복잡 한 기계 학습 모델을 학습 하거나 데이터를 조작 하는 스크립트를 사용할 수 있습니다.

ML Studio 실험에서 R을 사용 하 여 철저 한 소개의 경우 체크 아웃 [R 프로그래밍 언어에서 Azure Machine Learning Studio를 사용 하 여 시작](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart)합니다.

### <a name="create-manage-and-deploy-experiments-from-your-local-r-environment"></a>로컬 R 환경에서 실험 생성, 관리 및 배포

Azure Machine Learning Studio를 사용 하 여 R을 사용할 수 있는 다른 방법으로 사용 하는 것은 [ `AzureML` ](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) 패키지를 모니터링 하 고 R 프로그래밍 환경과 실험 프로세스를 제어 합니다.  Microsoft에서 유지 관리,이 패키지를 업로드 하 고 실험을 조사 하는 데 Azure Machine Learning Studio에서 데이터 집합을 다운로드할 수 있습니다 R 게시할 웹 서비스로 함수와 R 실행을 통해 기존 데이터를 웹 서비스를 검색 합니다 출력입니다.

이 패키지는 확장성 있는 배포 플랫폼으로 Azure Machine Learning Studio를 사용 하 여 R 코드를 훨씬 쉽게 수 있습니다.  UI에서 클릭하고 끄는 대신 이미 알고 있는 도구를 사용하여 전체적인 배포 프로세스를 자동화할 수 있습니다.

## <a name="azure-batch"></a>Azure Batch

대규모 R 작업의 경우 [Azure Batch](https://azure.microsoft.com/services/batch/)를 사용할 수 있습니다.  이 서비스는 클라우드 규모의 작업 예약 및 컴퓨팅 관리를 제공하므로 수십, 수백 또는 수천 대의 가상 머신에서 R 워크로드를 확장할 수 있습니다.  일반화된 컴퓨팅 플랫폼이므로 Azure Batch에서 R 작업을 실행하기 위한 몇 가지 옵션이 있습니다.

Microsoft의를 사용 하는 한 가지 방법은 [ `doAzureParallel` ](https://github.com/Azure/doAzureParallel) 패키지 있습니다.  이 R 패키지는 `foreach` 패키지의 병렬 백 엔드입니다.  각`foreach` 루프 반복이 Azure Batch 클러스터 내 노드에서 동시에 실행될 수 있게 해줍니다.  패키지에 소개 블로그 게시물을 참조 하세요. [doAzureParallel: R 세션에서 직접 Azure의 유연한 계산 활용](https://azure.microsoft.com/blog/doazureparallel/)합니다.

Azure Batch에서 R 스크립트를 실행하기 위한 다른 옵션은 Azure Portal에서 "RScript.exe"를 사용하여 코드를 Batch 앱의 번들로 묶는 것입니다.  자세한 연습을 참조 하세요 [Azure Batch에서 R 워크 로드](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/)합니다.

세 번째 옵션은 Azure Batch에서 Docker 컨테이너를 사용하여 요청 시 Spark 클러스터를 프로비전할 수 있게 해주는 [Azure Distributed Data Engineering Toolkit](https://github.com/Azure/aztk)(AZTK)을 사용하는 것입니다.  이는 Azure에서 Spark 작업을 실행하는 경제적인 방법을 제공합니다.  [AZTK를 지원하는 SparklyR](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK)을 사용하면 클라우드에서 쉽고 저렴하게 R 스크립트를 확장할 수 있습니다.

## <a name="azure-notebooks"></a>Azure Notebooks

[Azure Notebooks](https://notebooks.azure.com)는 노트북을 사용하여 Azure에 코드를 작성하는 것을 선호하는 R 개발자를 위한 무난한 저가형 서비스입니다.  markdown 문서, 실행 가능한 코드 및 그래픽을 단일 캔버스에 결합하는 데 사용되는 오픈 소스 프로젝트인 [Jupyter](https://jupyter.org/)를 사용하여 브라우저에서 코드를 개발하고 실행할 수 있게 해주는 무료 서비스입니다.

Azure Notebooks의 무료 서비스 계층은 각 Notebook의 처리량이 메모리 4GB 및 데이터 세트 1GB로 제한되므로 소규모 프로젝트에 적합한 옵션입니다. 하지만 이러한 제한보다 높은 컴퓨팅 및 데이터 성능이 필요한 경우 Data Science Virtual Machine 인스턴스에서 Notebook을 실행하면 됩니다. 자세한 내용은 [Azure Notebooks 프로젝트 관리 및 구성 - 컴퓨팅 계층](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier)을 참조하세요.

## <a name="azure-sql-database"></a>Azure SQL Database

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/)는 Microsoft의 완전히 관리되는 완전 지능형의 관계형 클라우드 데이터베이스 서비스입니다.  번거롭게 인프라를 설정하지 않고도 SQL Server의 모든 기능을 사용할 수 있게 해줍니다.  여기에 [SQL server에서 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017), SQL의 최신 추가 기능 중 하나입니다.

이 기능은 SQL Server 데이터베이스 내에서 저장 프로시저로 실행하거나, R 문이 포함된 T-SQL 스크립트로 실행하거나, T-SQL이 포함된 R 코드로 실행할 수 있는 예측 분석 및 데이터 과학 엔진을 기본 제공합니다.  데이터베이스에서 데이터를 추출하고 R 환경으로 로드하는 대신 데이터베이스에 직접 R 코드를 로드하고 데이터와 함께 바로 실행할 수 있게 해줍니다.

Machine Learning Services는 2016년부터 온-프레미스 SQL Server에 포함되었지만 Azure SQL Database에는 비교적 최근에 추가되었습니다.  현재 [제한된 미리 보기](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap) 상태이지만 계속 개선될 예정입니다.


### <a name="next-steps"></a>다음 단계

* [Mrsdeploy를 사용 하 여 Azure에서 R 코드 실행](https://blog.revolutionanalytics.com/2017/03/running-your-r-code-azure.html)
* [Machine Learning Server 클라우드에서](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-in-the-cloud)
* [Machine Learning Server 및 Microsoft R에 대 한 추가 리소스](https://docs.microsoft.com/machine-learning-server/resources-more)
* [Azure의 R](https://github.com/yueguoguo/r-on-azure) - Azure에서 R을 사용하는 데 필요한 패키지, 도구 및 사례 연구에 대한 개요

---

<sub>R 로고는 &copy; The R Foundation 2016 조건으로 사용 되 고는 [Creative Commons Attribution-sharealike 4.0 International 라이선스](https://creativecommons.org/licenses/by-sa/4.0/)합니다.</sub>
