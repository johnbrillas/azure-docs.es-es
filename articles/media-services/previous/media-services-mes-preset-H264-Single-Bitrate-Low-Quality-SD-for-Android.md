---
title: H264 Single Bitrate Low Quality SD for Android | Microsoft Docs
description: El tema proporciona información general sobre el valor predeterminado de la tarea **H264 Single Bitrate Low Quality SD for Android**.
author: IngridAtMicrosoft
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 67d3446d-e3b8-419f-bbf8-e5149c108531
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 4c32fc01b2011d49dc50a67edc98227f5b31750c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103011721"
---
# <a name="h264-single-bitrate-low-quality-sd-for-android"></a>H264 Single Bitrate Low Quality SD for Android

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

`Media Encoder Standard` define un conjunto de valores predeterminados de codificación que puede usar al crear trabajos de codificación. Puede usar `preset name` para especificar en qué formato desea codificar el archivo multimedia. O bien, puede crear sus propios valores predeterminados basados en XML o JSON (mediante la codificación UTF-8 o UTF-16). Después pasaría el valor predeterminado personalizado al codificador. Para obtener la lista de todos los nombres predeterminados admitidos por este codificador `Media Encoder Standard`, vea [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md) (Valores predeterminados de tarea para Media Encoder Standard).  
  
 En este tema se muestra el valor predeterminado `H264 Single Bitrate Low Quality SD for Android` en formato XML y JSON.  
  
 Este valor predeterminado genera un único archivo MP4 con una velocidad de bits de 56 kbps y audio AAC estéreo. Para información detallada sobre el perfil, la velocidad de bits, la frecuencia de muestreo, etc. de este valor predeterminado, examine el código XML o JSON definido más adelante. Para obtener explicaciones de lo que cada elemento significa en estos nombres predeterminados y los valores válidos para cada elemento, vea el tema sobre el [esquema de Media Encoder Standard](media-services-mes-schema.md).  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:05</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>56</Bitrate>  
          <Width>176</Width>  
          <Height>144</Height>  
          <FrameRate>12/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>2</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>56</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>HEAACV2</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>24</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:05",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Baseline",  
          "Level": "2",  
          "Bitrate": 56,  
          "MaxBitrate": 56,  
          "BufferWindow": "00:00:05",  
          "Width": 176,  
          "Height": 144,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "12/1"  
        }  
      ],  
      "Type": "H264Video"  
    },  
    {  
      "Profile": "HEAACV2",  
      "Channels": 2,  
      "SamplingRate": 48000,  
      "Bitrate": 24,  
      "Type": "AACAudio"  
    }  
  ],  
  "Outputs": [  
    {  
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
      "Format": {  
        "Type": "MP4Format"  
      }  
    }  
  ]  
}  
```
