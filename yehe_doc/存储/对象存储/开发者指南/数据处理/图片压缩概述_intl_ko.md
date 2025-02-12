## 개요

이미지 압축이란 이미지의 저장 및 트래픽 비용을 줄이고 액세스 속도를 높이기 위해 품질을 변경하지 않고 이미지의 크기를 최대한 줄이는 것을 말합니다.

Cloud Object Storage(COS)는 비즈니스 시나리오에 따라 [Cloud Infinite(CI)](https://intl.cloud.tencent.com/document/product/1045/33422) 기반의 다양한 이미지 압축 방식을 출시했습니다. 현재 지원되는 압축 방법은 다음과 같습니다.

- **WebP 압축:** 압축 측면에서 jpg보다 우수한 webp 형식으로 이미지를 변환합니다. webp 이미지는 동일한 품질의 jpg 이미지보다 25% 이상 작습니다. 이 형식은 다중 터미널 사용 사례에 적합합니다.
- **HEIF 압축:** 이미지를 압축률이 매우 높은 heif 형식으로 변환합니다. webp 이미지는 동일한 품질의 jpg 이미지보다 80% 이상 작습니다. iOS는 heif를 사진의 기본 형식으로 채택하고 Android P는 기본적으로 heif를 지원합니다.
- **TPG 압축:** 이미지를 tpg 형식으로 변환합니다. 이 형식은 Tencent에서 출시한 독점 이미지 형식이며 애니메이션 이미지를 지원합니다. 현재 QQ Browser, Qzone 및 기타 Tencent 제품은 기본적으로 tpg를 지원합니다. tpg 이미지는 동일한 품질의 gif 또는 png 이미지보다 각각 90% 이상 또는 50% 이상 작습니다.
- **AVIF 압축:** 2020년 2월 av1 기반으로 Netflix에서 출시한 새로운 이미지 형식인 avif 형식으로 이미지를 변환하며 현재 Chrome 및 Firefox와 같은 브라우저에서 지원됩니다.

>? 이미지 압축은 CI에서 청구하는 유료 서비스입니다. 자세한 가격은 CI의 이미지 처리 가격을 참고하십시오.
>

## 적용 시나리오

이미지 압축 기능은 전자 상거래 및 미디어와 같은 다양한 사용 사례에서 PC 및 App과 같은 다양한 터미널에서 이미지 압축 요구를 충족합니다. 이는 전송 시간, 로딩 시간, 대역폭 및 트래픽 사용을 효과적으로 줄입니다.

다양한 압축 기능은 아래에 설명된 대로 기존 이미지 형식 및 브라우저 환경과의 호환성이 다릅니다.

| 기능      | 지원 형식                                                     | 지원 브라우저 및 시스템                                            | 호환성 | 압축 효과 | 압축 속도 |
| :-------- | :----------------------------------------------------------- | ------------------------------------------------------------ | ------ | -------- | -------- |
| WebP 압축 | jpg, png, bmp, gif, heif, tpg, avif. | Edge, Firefox, Chrome, Safari, Android, iOS 및 WeChat과 같은 브라우저 및 시스템의 95% 이상 | 강함     | 평균     | 빠름       |
| HEIF 압축 | jpg, png, bmp, webp, avif.      | 브라우저에서는 지원되지 않지만 iOS 11 이상 및 Android P에서 기본적으로 지원됨      | 약함     | 강함       | 빠름       |
| TPG 압축  | jpg, png, bmp, gif, heif, webp, avif. | 특수 디코더로 QQ 브라우저와 같은 몇 가지 브라우저만 필요                   | 약함     | 강함       | 빠름       |
| AVIF 압축 | jpg, png, bmp, gif, heif, webp, tpg. | Firefox, Chrome 및 Android와 같은 일부 브라우저 및 시스템               | 약함     | 매우 강함     | 느림       |


>? TPG 형식을 사용하려면 **이미지가 로드되는 환경이 TPG 디코딩을 지원**하는지 확인하십시오. CI는 [iOS](https://intl.cloud.tencent.com/document/product/1045/47707), [Android](https://intl.cloud.tencent.com/document/product/1045/45575) 및 [Windows](https://main.qcloudimg.com/raw/851dd252378813d250eeca5ed55ffd36/TPG_win_SDK.zip)용 TPG 디코더 통합 SDK를 제공하여 TPG와의 빠른 통합을 용이하게 합니다.
>

## 사용 방법

기본 이미지 처리의 형식 변환 기능을 통해 위의 압축 기능을 사용할 수 있습니다. 이렇게 하려면 format 매개변수를 원하는 압축 형식으로 설정하십시오. 특정 매개변수는 다음과 같습니다.

| 매개변수 | 설명                                   |
| :----- | :----------------------------------------- |
| webp   | 압축을 위해 원본 이미지를 webp 형식으로 변환합니다. |
| heif   | 압축을 위해 원본 이미지를 heif 형식으로 변환합니다. |
| tpg    | 압축을 위해 원본 이미지를 tpg 형식으로 변환합니다.  |
| avif   | 압축을 위해 원본 이미지를 avif 형식으로 변환합니다. |

## 압축 예시

다음 예시에서는 `https://ci-avif-demo-1258125638.cos.ap-guangzhou.myqcloud.com/test-png.png`에서 png 형식의 원본 이미지를 각각 jpeg, webp, heif, tpg 및 avif 형식으로 변환합니다. 효과는 다음과 같습니다:

![img](https://ci-avif-demo-1258125638.cos.ap-guangzhou.myqcloud.com/test-png.png)

### 예시1: jpeg 형식으로 변환

최종 요청 URL:
```
https://ci-avif-demo-1258125638.cos.ap-guangzhou.myqcloud.com/test-png.png?imageMogr2/format/jpeg
```

### 예시2: webp 형식으로 변환

최종 요청 URL:

```
https://ci-avif-demo-1258125638.cos.ap-guangzhou.myqcloud.com/test-png.png?imageMogr2/format/webp
```

### 예시3: heif 형식으로 변환

최종 요청 URL:

```
https://ci-avif-demo-1258125638.cos.ap-guangzhou.myqcloud.com/test-png.png?imageMogr2/format/heif
```

### 예시4: tpg 형식으로 변환

최종 요청 URL:

```
https://ci-avif-demo-1258125638.cos.ap-guangzhou.myqcloud.com/test-png.png?imageMogr2/format/tpg
```

### 예시5: avif 형식으로 변환

최종 요청 URL:

```
https://ci-avif-demo-1258125638.cos.ap-guangzhou.myqcloud.com/test-png.png?imageMogr2/format/avif
```

다음 표에서는 다양한 이미지 압축 방법의 압축률을 비교합니다:

| 형식        | 크기               |
| :---------- | :----------------- |
| png(원본 이미지) | 465 KB             |
| jpeg        | 114KB(75.5% 더 작음) |
| webp        | 64 KB(86.2% 더 작음) |
| heif        | 54 KB(88.4% 더 작음) |
| tpg         | 56 KB(88.0% 더 작음) |
| avif        | 32 KB(93.1% 더 작음) |

