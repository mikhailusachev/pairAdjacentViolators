# Pair Adjacent Violators

## Overview

An implementation of the [Pair Adjacent Violators](http://gifi.stat.ucla.edu/janspubs/2009/reports/deleeuw_hornik_mair_R_09.pdf) algorithm for [isotonic regression](https://en.wikipedia.org/wiki/Isotonic_regression).  Written in [Kotlin](http://kotlinlang.org/) but usable from Java or any [other JVM language](https://en.wikipedia.org/wiki/List_of_JVM_languages).  

Note this algorithm is also known as "Pool Adjacent Violators".

While not widely known, I've found this algorithm useful in a variety of circumstances, particularly when it comes to [calibration of predictive model outputs](http://scikit-learn.org/stable/modules/calibration.html).

A picture is worth a thousand words:

![PAV in action](https://sanity.github.io/pairAdjacentViolators/pav-example.png)

## Features

* Tries to do one thing and do it well with minimal bloat, no external dependencies (other than Kotlin's stdlib)
* Fairly comprehensive [unit tests](https://github.com/trystacks/pairAdjacentViolators/tree/master/src/test/kotlin/com/trystacks/pav) (using [Kotlintest](https://github.com/kotlintest/kotlintest))
* Employs an isotonic spline algorithm for smooth interpolation
* Fairly efficient implementation without compromizing code readability
* While implemented in Kotlin, should be usable from Java, Clojure, Scala, and other JVM languages
* Supports reverse-interpolation

## Limitations

* Very little documentation for the moment, however your IDE should be able to assist for now, and you can always read the source!

## Usage

### Adding library dependency

You can use this library by adding a dependency for Gradle, Maven, SBT, Leiningen or another Maven-compatible dependency management system thanks to Jitpack:

[![](https://jitpack.io/v/sanity/pairAdjacentViolators.svg)](https://jitpack.io/#sanity/pairAdjacentViolators)

### Basic usage from Kotlin

```kotlin
import com.trystacks.pav.PairAdjacentViolators
import com.trystacks.pav.PairAdjacentViolators.*
// ...
val inputPoints = listOf(Point(3.0, 1.0), Point(4.0, 2.0), Point(5.0, 3.0), Point(8.0, 4.0))
val pav = PairAdjacentViolators(inputPoints)
val interpolator = pav.interpolator()
println("Interpolated: ${interpolator(6.0)}")
```

### Basic usage from Java
```java
import com.trystacks.pav.*;
import com.trystacks.pav.PairAdjacentViolators.*;
import kotlin.jvm.functions.*;
import java.util.*;

public class PAVTest {
    public static void main(String[] args) {
        List<Point> points = new LinkedList<>();
        points.add(new Point(0.0, 0.0, 1.0));
        points.add(new Point(1.0, 1.0, 1.0));
        points.add(new Point(2.0, 3.0, 1.0));
        points.add(new Point(3.0, 5.0, 1.0));
        PairAdjacentViolators pav = new PairAdjacentViolators(points, PAVMode.INCREASING);
        final Function1<Double, Double> interpolator = pav.interpolator(InterpolationStrategy.SPLINE);
        for (double x=0; x<3; x+=0.1) {
            System.out.println(x+"\t"+interpolator.invoke(x));
        }
    }
}
```

### License
Released under the [LGPL](https://en.wikipedia.org/wiki/GNU_Lesser_General_Public_License) version 3 by [Ian Clarke](http://blog.locut.us/) of [Stacks](http://trystacks.com/).

### Development
[![Build Status](https://travis-ci.org/sanity/pairAdjacentViolators.svg?branch=master)](https://travis-ci.org/sanity/pairAdjacentViolators)
