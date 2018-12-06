---
title: 자연어 처리 기술 선택
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.openlocfilehash: b1cb019164285d16b6e9d34eae220801785adab9
ms.sourcegitcommit: e7e0e0282fa93f0063da3b57128ade395a9c1ef9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52902311"
---
# <a name="choosing-a-natural-language-processing-technology-in-azure"></a><span data-ttu-id="a528e-102">Azure에서 자연어 처리 기술 선택</span><span class="sxs-lookup"><span data-stu-id="a528e-102">Choosing a natural language processing technology in Azure</span></span>

<span data-ttu-id="a528e-103">자유 형식 텍스트 처리는 일반적으로 검색을 지원하기 위해 텍스트 단락을 포함하는 문서에 대해 수행되지만, 감성 분석, 토픽 감지, 언어 감지, 핵심 구 추출 및 문서 분류와 같은 기타 NLP(자연어 처리) 작업을 수행하는 데도 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-103">Free-form text processing is performed against documents containing paragraphs of text, typically for the purpose of supporting search, but is also used to perform other natural language processing (NLP) tasks such as sentiment analysis, topic detection, language detection, key phrase extraction, and document categorization.</span></span> <span data-ttu-id="a528e-104">이 문서에서는 NLP 작업을 지원하는 기술 선택 사항을 집중적으로 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-104">This article focuses on the technology choices that act in support of the NLP tasks.</span></span>

## <a name="what-are-your-options-when-choosing-an-nlp-service"></a><span data-ttu-id="a528e-105">NLP 서비스를 선택할 때 사용할 수 있는 옵션은 무엇인가요?</span><span class="sxs-lookup"><span data-stu-id="a528e-105">What are your options when choosing an NLP service?</span></span>

<span data-ttu-id="a528e-106">Azure에서 다음 서비스는 NLP(자연어 처리) 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-106">In Azure, the following services provide natural language processing (NLP) capabilities:</span></span>

- [<span data-ttu-id="a528e-107">Azure HDInsight(Spark 및 Spark MLlib 포함)</span><span class="sxs-lookup"><span data-stu-id="a528e-107">Azure HDInsight with Spark and Spark MLlib</span></span>](/azure/hdinsight/spark/apache-spark-overview)
- [<span data-ttu-id="a528e-108">Azure Databricks</span><span class="sxs-lookup"><span data-stu-id="a528e-108">Azure Databricks</span></span>](/azure/azure-databricks/what-is-azure-databricks)
- [<span data-ttu-id="a528e-109">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="a528e-109">Microsoft Cognitive Services</span></span>](/azure/cognitive-services/welcome)

## <a name="key-selection-criteria"></a><span data-ttu-id="a528e-110">주요 선택 조건</span><span class="sxs-lookup"><span data-stu-id="a528e-110">Key selection criteria</span></span>

<span data-ttu-id="a528e-111">선택 옵션의 범위를 좁히려면 먼저 다음 질문에 답변합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-111">To narrow the choices, start by answering these questions:</span></span>

- <span data-ttu-id="a528e-112">미리 작성된 모델을 사용하려고 하나요?</span><span class="sxs-lookup"><span data-stu-id="a528e-112">Do you want to use prebuilt models?</span></span> <span data-ttu-id="a528e-113">그렇다면 Microsoft Cognitive Services에서 제공하는 API를 사용하는 것을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-113">If yes, consider using the APIs offered by Microsoft Cognitive Services.</span></span>

- <span data-ttu-id="a528e-114">대량의 텍스트 데이터에 대해 사용자 지정 모델을 학습해야 하나요?</span><span class="sxs-lookup"><span data-stu-id="a528e-114">Do you need to train custom models against a large corpus of text data?</span></span> <span data-ttu-id="a528e-115">그렇다면 Spark MLlib 및 Spark NLP와 함께 Azure HDInsight를 사용하는 것을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-115">If yes, consider using Azure HDInsight with Spark MLlib and Spark NLP.</span></span>

- <span data-ttu-id="a528e-116">토큰화, 형태소 분석, 기본형 찾기 및 TF/IDF(용어 빈도/역 문서 빈도) 같은 기본적인 NLP 기능이 필요한가요?</span><span class="sxs-lookup"><span data-stu-id="a528e-116">Do you need low-level NLP capabilities like tokenization, stemming, lemmatization, and term frequency/inverse document frequency (TF/IDF)?</span></span> <span data-ttu-id="a528e-117">그렇다면 Spark MLlib 및 Spark NLP와 함께 Azure HDInsight를 사용하는 것을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-117">If yes, consider using Azure HDInsight with Spark MLlib and Spark NLP.</span></span>

- <span data-ttu-id="a528e-118">엔터티 및 의도 식별, 토픽 감지, 맞춤법 검사 또는 감성 분석 같은 간단한 고급 NLP 기능이 필요한가요?</span><span class="sxs-lookup"><span data-stu-id="a528e-118">Do you need simple, high-level NLP capabilities like entity and intent identification, topic detection, spell check, or sentiment analysis?</span></span> <span data-ttu-id="a528e-119">그렇다면 Microsoft Cognitive Services에서 제공하는 API를 사용하는 것을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-119">If yes, consider using the APIs offered by Microsoft Cognitive Services.</span></span>

## <a name="capability-matrix"></a><span data-ttu-id="a528e-120">기능 매트릭스</span><span class="sxs-lookup"><span data-stu-id="a528e-120">Capability matrix</span></span>

<span data-ttu-id="a528e-121">다음 표에서는 주요 기능 차이점을 요약해서 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-121">The following tables summarize the key differences in capabilities.</span></span>  

### <a name="general-capabilities"></a><span data-ttu-id="a528e-122">일반 기능</span><span class="sxs-lookup"><span data-stu-id="a528e-122">General capabilities</span></span>

| | <span data-ttu-id="a528e-123">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a528e-123">Azure HDInsight</span></span> | <span data-ttu-id="a528e-124">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="a528e-124">Microsoft Cognitive Services</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a528e-125">미리 학습된 모델을 서비스로 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-125">Provides pretrained models as a service</span></span> | <span data-ttu-id="a528e-126">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-126">No</span></span> | <span data-ttu-id="a528e-127">yes</span><span class="sxs-lookup"><span data-stu-id="a528e-127">Yes</span></span> |
| <span data-ttu-id="a528e-128">REST API</span><span class="sxs-lookup"><span data-stu-id="a528e-128">REST API</span></span> | <span data-ttu-id="a528e-129">yes</span><span class="sxs-lookup"><span data-stu-id="a528e-129">Yes</span></span> | <span data-ttu-id="a528e-130">yes</span><span class="sxs-lookup"><span data-stu-id="a528e-130">Yes</span></span> |
| <span data-ttu-id="a528e-131">프로그래밍 기능</span><span class="sxs-lookup"><span data-stu-id="a528e-131">Programmability</span></span> | <span data-ttu-id="a528e-132">Python, Scala, Java</span><span class="sxs-lookup"><span data-stu-id="a528e-132">Python, Scala, Java</span></span> | <span data-ttu-id="a528e-133">C#, Java, Node.js, Python, PHP, Ruby</span><span class="sxs-lookup"><span data-stu-id="a528e-133">C#, Java, Node.js, Python, PHP, Ruby</span></span> |
| <span data-ttu-id="a528e-134">빅 데이터 집합 및 대형 문서의 처리를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="a528e-134">Support processing of big data sets and large documents</span></span> | <span data-ttu-id="a528e-135">yes</span><span class="sxs-lookup"><span data-stu-id="a528e-135">Yes</span></span> | <span data-ttu-id="a528e-136">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-136">No</span></span> |

### <a name="low-level-natural-language-processing-capabilities"></a><span data-ttu-id="a528e-137">기본적인 자연어 처리 기능</span><span class="sxs-lookup"><span data-stu-id="a528e-137">Low-level natural language processing capabilities</span></span>

| | <span data-ttu-id="a528e-138">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a528e-138">Azure HDInsight</span></span> | <span data-ttu-id="a528e-139">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="a528e-139">Microsoft Cognitive Services</span></span> |  
| --- | --- | --- | 
| <span data-ttu-id="a528e-140">토크나이저</span><span class="sxs-lookup"><span data-stu-id="a528e-140">Tokenizer</span></span> | <span data-ttu-id="a528e-141">예(Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="a528e-141">Yes (Spark NLP)</span></span> | <span data-ttu-id="a528e-142">예(Linguistic Analysis API)</span><span class="sxs-lookup"><span data-stu-id="a528e-142">Yes (Linguistic Analysis API)</span></span> |
| <span data-ttu-id="a528e-143">형태소 분석기</span><span class="sxs-lookup"><span data-stu-id="a528e-143">Stemmer</span></span> | <span data-ttu-id="a528e-144">예(Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="a528e-144">Yes (Spark NLP)</span></span> | <span data-ttu-id="a528e-145">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-145">No</span></span> |
| <span data-ttu-id="a528e-146">기본형 분석기</span><span class="sxs-lookup"><span data-stu-id="a528e-146">Lemmatizer</span></span> | <span data-ttu-id="a528e-147">예(Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="a528e-147">Yes (Spark NLP)</span></span> | <span data-ttu-id="a528e-148">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-148">No</span></span> |
| <span data-ttu-id="a528e-149">품사 태그 지정</span><span class="sxs-lookup"><span data-stu-id="a528e-149">Part of speech tagging</span></span> | <span data-ttu-id="a528e-150">예(Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="a528e-150">Yes (Spark NLP)</span></span> | <span data-ttu-id="a528e-151">예(Linguistic Analysis API)</span><span class="sxs-lookup"><span data-stu-id="a528e-151">Yes (Linguistic Analysis API)</span></span> |
| <span data-ttu-id="a528e-152">TF/IDF(용어 빈도/역 문서 빈도)</span><span class="sxs-lookup"><span data-stu-id="a528e-152">Term frequency/inverse-document frequency (TF/IDF)</span></span> | <span data-ttu-id="a528e-153">예(Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="a528e-153">Yes (Spark MLlib)</span></span> | <span data-ttu-id="a528e-154">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-154">No</span></span> |
| <span data-ttu-id="a528e-155">문자열 유사성&mdash;편집 거리 계산</span><span class="sxs-lookup"><span data-stu-id="a528e-155">String similarity&mdash;edit distance calculation</span></span> | <span data-ttu-id="a528e-156">예(Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="a528e-156">Yes (Spark MLlib)</span></span> | <span data-ttu-id="a528e-157">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-157">No</span></span> |
| <span data-ttu-id="a528e-158">N-gram 계산</span><span class="sxs-lookup"><span data-stu-id="a528e-158">N-gram calculation</span></span> | <span data-ttu-id="a528e-159">예(Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="a528e-159">Yes (Spark MLlib)</span></span> | <span data-ttu-id="a528e-160">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-160">No</span></span> |
| <span data-ttu-id="a528e-161">중지 단어 제거</span><span class="sxs-lookup"><span data-stu-id="a528e-161">Stop word removal</span></span> | <span data-ttu-id="a528e-162">예(Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="a528e-162">Yes (Spark MLlib)</span></span> | <span data-ttu-id="a528e-163">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-163">No</span></span> |

### <a name="high-level-natural-language-processing-capabilities"></a><span data-ttu-id="a528e-164">고급 자연어 처리 기능</span><span class="sxs-lookup"><span data-stu-id="a528e-164">High-level natural language processing capabilities</span></span>

| | <span data-ttu-id="a528e-165">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a528e-165">Azure HDInsight</span></span> | <span data-ttu-id="a528e-166">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="a528e-166">Microsoft Cognitive Services</span></span> |
| --- | --- | --- | 
| <span data-ttu-id="a528e-167">엔터티/의도 식별 및 추출</span><span class="sxs-lookup"><span data-stu-id="a528e-167">Entity/intent identification & extraction</span></span> | <span data-ttu-id="a528e-168">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-168">No</span></span> | <span data-ttu-id="a528e-169">예(LUIS(언어 인식 인텔리전트 서비스) API)</span><span class="sxs-lookup"><span data-stu-id="a528e-169">Yes (Language Understanding Intelligent Service (LUIS) API)</span></span> |    
| <span data-ttu-id="a528e-170">토픽 검색</span><span class="sxs-lookup"><span data-stu-id="a528e-170">Topic detection</span></span> | <span data-ttu-id="a528e-171">예(Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="a528e-171">Yes (Spark NLP)</span></span> | <span data-ttu-id="a528e-172">예(Text Analytics API)</span><span class="sxs-lookup"><span data-stu-id="a528e-172">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="a528e-173">맞춤법 검사</span><span class="sxs-lookup"><span data-stu-id="a528e-173">Spell checking</span></span> | <span data-ttu-id="a528e-174">예(Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="a528e-174">Yes (Spark NLP)</span></span> | <span data-ttu-id="a528e-175">예(Bing Spell Check API)</span><span class="sxs-lookup"><span data-stu-id="a528e-175">Yes (Bing Spell Check API)</span></span> |
| <span data-ttu-id="a528e-176">정서 분석</span><span class="sxs-lookup"><span data-stu-id="a528e-176">Sentiment analysis</span></span> | <span data-ttu-id="a528e-177">예(Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="a528e-177">Yes (Spark NLP)</span></span> | <span data-ttu-id="a528e-178">예(Text Analytics API)</span><span class="sxs-lookup"><span data-stu-id="a528e-178">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="a528e-179">언어 검색</span><span class="sxs-lookup"><span data-stu-id="a528e-179">Language detection</span></span> | <span data-ttu-id="a528e-180">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-180">No</span></span> | <span data-ttu-id="a528e-181">예(Text Analytics API)</span><span class="sxs-lookup"><span data-stu-id="a528e-181">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="a528e-182">영어 이외의 다국어 지원</span><span class="sxs-lookup"><span data-stu-id="a528e-182">Supports multiple languages besides English</span></span> | <span data-ttu-id="a528e-183">아니요</span><span class="sxs-lookup"><span data-stu-id="a528e-183">No</span></span> | <span data-ttu-id="a528e-184">예(API에 따라 다름)</span><span class="sxs-lookup"><span data-stu-id="a528e-184">Yes (varies by API)</span></span> |

## <a name="see-also"></a><span data-ttu-id="a528e-185">참고 항목</span><span class="sxs-lookup"><span data-stu-id="a528e-185">See also</span></span>

[<span data-ttu-id="a528e-186">자연어 처리</span><span class="sxs-lookup"><span data-stu-id="a528e-186">Natural language processing</span></span>](../scenarios/natural-language-processing.md)
