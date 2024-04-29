---
title: "Developing an Object Oriented Raytracer in C++"
date: 2020-08-21T21:15:04+02:00
description: "Developing a Raytracer in C++ using Object Oriented Programming, with support for multiple types of objects, diffuse and reflection, multiple light sources and a scene reader."
categories: ["3D-rendering"]
tags: ["C++", "Object Oriented Programming"]
aliases: ["/2020/08/ray-tracer/", "/blog/3d-rendering/developing-an-object-oriented-raytracer-in-c-plus-plus/"]
---

For a programming class during my studies at the Rotterdam University of Applied Sciences I wrote an object oriented ray tracer in C++. There is support for multiple types of objects, diffuse and reflection, multiple light sources and a scene reader.

I have rendered the same scene with different resolutions and samples per pixel. Samples per pixel refers to the number of times a ray is shot through every pixel. For every sample, a ray is shot through a slightly different point in the pixel.

  {{< figure src="/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_500_500_10.png"    caption="Sampels per pixel: 10" >}}
  {{< figure src="/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_500_500_100.png"   caption="Sampels per pixel: 100" >}}
  {{< figure src="/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_500_500_1000.png"  caption="Sampels per pixel: 1000" >}}
  {{< figure src="/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_500_500_10000.png" caption="Sampels per pixel: 10000" >}}

## Renders
Render time for the cornell box on a i5-8250u processor. 

| Resolution    | Sampels Per Pixel| Render time| Result                                                          |
| ------------- |------------------| -----------|-----------------------------------------------------------------|
| 100x100       | 10               | 0.27s      |[Link](/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_100_100_10.png)    |
| 100x100       | 100              | 2.32s      |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_100_100_100.png)   |
| 100x100       | 1000             | 23.93s     |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_100_100_1000.png)  |
| 100x100       | 10000            | 4m34s      |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_100_100_10000.png) |
| 500x500       | 10               | 6.38s      |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_500_500_10.png)    |
| 500x500       | 100              | 1m7s       |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_500_500_100.png)   |
| 500x500       | 1000             | 11m27s     |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_500_500_1000.png)  |
| 500x500       | 10000            | 1h54m36s   |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_500_500_10000.png) |
| 1000x1000     | 10               | 28.16s     |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_1000_1000_10.png)  |
| 1000x1000     | 100              | 4m41s      |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_1000_1000_100.png) |
| 1000x1000     | 1000             | 45m58s     |[Link](/assets/img/posts/developing-an-object-oriented-raytracer-in-c-plus-plus/image_1000_1000_1000.png)|

## Features
* Support for Spheres, Cubes and Planes
* Diffuse and reflection
* Multiple lights
* Movable camera with a user controlled Field of View (FoV) and position
* Scene reader using JSON

## Architecture

The ray tracer is build using Object-oriented programming (OOP). For example, a parent class (BasicObject) was created for all objects to be placed in the scene. The different objects are derived from this parent class (Cube, Sphere and Plane). By using the Parent-Child structure, the child class can inherit functions that are the same for all objects. The full UML can be found [here](https://raw.githubusercontent.com/niekvleeuwen/ray-tracer/master/UML.png). A simplified UML can be found below.

{{< figure src="/assets/img/portfolio/ray-tracer/design.png" caption="Simplified UML" >}}

## Code & Installation

The source code for the Ray Tracer is available [here](https://github.com/niekvleeuwen/ray-tracer). The installation process is quite simple. Clone the repository and execute the following command:

```shell
$ make
```