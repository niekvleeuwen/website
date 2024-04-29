---
title: "Measuring Premature Born Babies Using 3D Cameras"
date: 2022-09-11T11:16:18+02:00
description: "Creating a 3D scanner to measure premature born babies lying in an incubator."
categories: ["3D Rendering"]
tags: ["C++", "Object Oriented Programming"]
aliases: ["/2022/06/using-3d-cameras-to-measure-premature-born-babies/", "/blog/3d-rendering/measuring-premature-born-babies-using-3d-cameras/"]
---


During my graduation internship at the Erasmus Medical Center I developed a 3D scanner to measure the body length, head circumference and cranial volume of premature born babies.

## Problem

In the Netherlands, around 12,000 babies are born premature each year. These premature babies are nursed in a Neonatal Intensive Care Unit (NICU) in an incubator, which provides a controlled environment. A premature baby needs optimal growth to promote neurological development in both the short and long term and is therefore measured weekly. To measure height, a fold-out ruler is used, which requires the premature infant to be stretched. To measure the head circumference, a tape measure is used, which is brought around the head. The current measuring instruments cause stress because the premature is touched. Stress, caused by discomfort or pain, can cause negative health effects, such as growth deficiency or impaired cognitive ability. In addition to the negative health effects, current instruments are unhygienic, inaccurate, and inconvenient to use. 

The aim of this study was to develop a 3D scanner that measures body length, head circumference and cranial volume in a contactless manner. For this purpose, the following research question was formulated: how to measure premature born babies using 3D cameras? Currently, there is no measuring equipment that can accurately measure the volume of the cranium without removing the premature from the incubator, while such information can be used to better predict growth. This study is part of Ronald van Gils' PhD study, which aims to find a new technical method to measure the growth of babies born prematurely. 

## Method

To answer the research question, a literature review and experimental research were conducted. It was investigated which type of camera is most suitable, how the system can be validated and how the system can be made more accurate. Experimental research was used to determine the optimal arrangement of the cameras. 

## Result

Scanning of the premature is done by making three captures. These captures are made from different perspectives of the premature baby and are combined with routine care (e.g. changing the diaper). The developed 3D scanner is placed over the incubator and the nurse makes a capture with the push of a button. The seven Intel RealSense D415 cameras send depth and RGB images to the host computer, which are merged into a 3D model with RecFusion. After three captures are made, they are merged into one scan. From this scan, the measurement data is then derived. 

The end result of this study is a working Proof of Concept (PoC) of a 3D scanner that allows a premature born baby to be measured contactless with an accuracy of 0.90 mm. Several recommendations were made to improve the system. For example, the scans should be able to be merged in a user-friendly way.

Please sent me an [email](mailto:ik@niekvanleeuwen.nl) to receive the thesis (in dutch).