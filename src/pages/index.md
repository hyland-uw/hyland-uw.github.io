---
layout: "../layouts/Layout.astro"
title: "The Fast Inverse Square Root"
description: "A place for information about what became known as the Fast Inverse Square Root"
---

# The Fast Inverse Square Root

In 2001 an approximation of a reciprocal square root (y = 1/sqrt(x))
leaked to the internet, was attributed to a charismatic hacker, and
attained the kind of fame normally reserved for soap opera stars or
particularly photogenic zoo animals. The code made its way around the internet as a snippet, usually (often deliberately) unaccompanied by any explanation.

``` cpp
float Q_rsqrt(float number)
{
  long i;
  float x2, y;
  const float threehalfs = 1.5F;

  x2 = number * 0.5F;
  y  = number;
  i  = * ( long * ) &y;                       // evil floating point bit level hacking
  i  = 0x5f3759df - ( i >> 1 );               // what the fuck?
  y  = * ( float * ) &i;
  y  = y * ( threehalfs - ( x2 * y * y ) );   // 1st iteration
  // y  = y * ( threehalfs - ( x2 * y * y ) );   // 2nd iteration, this can be removed

  return y;
}
```

Perhaps this doesn't appear so interesting at first glance, but it's a bit extraordinary. Why?

Let's look at a method of appoximating the square root which you might have learned in school. The square root of x can be represented as a rational power of x, x<sup>(1/2)</sup> = sqrt(x). We can take logs of both sides to get ln(sqrt(x)) = ln(x<sup>(1/2)</sup>) = 1/2 * ln(x). The logarithm is the inverse of exponentiation, so exponentiating both sides gives you e<sup>((1/2) * ln(x))</sup> = sqrt(x).

Expressed as a procedure to compute the square root of x, you take the log of x, divide that by 2, then exponentiate the result. We replaced the task of finding roots with finding logs and exponents and dividing. Assuming we have access to a table of logarithms, this is a fairly straighforward approximation, but it both requires access to that table (and a more precise table offers better approximations) and a division. For a computer--human or otherwise--efficiently generating and accessing a table of logarithms is a challenge and for that same computer, division is historically a slow operation.

The FISR achieves the same schoolbook result in fundamentally three lines of code:

``` cpp
i  = * ( long * ) &y;
i  = 0x5f3759df - ( i >> 1 );
y  = * ( float * ) &i;
```

There is--apparently--no table of logarithms, no division, and no exponentiation. The code is a bit of a magic trick, and like any good magic trick, misdirection of attention is key. The integer constant `0x5f3759df` attracts much of that attention, especially when seen alongside the comment "`//what the fuck?`", but the real trick is hidden in the input type itself, floating point numbers.

This page is not, per se, an explanation of the Fast Inverse Square Root (though plenty are linked). Rather it is a history of the mystery behind the FISR and an attempt to build out our understanding of an approximation which has been shared, re-discovered, and adapted to many different context in the decades since the code leaked.

## Hang on, isn\'t the magic constant \"0x5F3759DF\"?

That\'s right! This page is named 0x5f37642f, Lomont\'s optimal constant
for the approximation derived **without** the next step of
Newton-Raphson (NR) in mind. Considering the next step of NR, Lomont\'s
constant is outperformed by 0x5F3759DF, which is the one that leaked to
the internet.

## Resources on Quake III\'s Fast Inverse Square Root

Resources are linked below where links are available and known live.
Otherwise copies are hosted on this site as fair use.

### Data and code on the FISR

This [repository](https://github.com/hyland-uw/FISR-historical) contains
code to reproduce versions of the FISR. Also contains data and R code to
plot data.

### Whitepapers about the Quake III FISR

-   [First explanation in English by Chris
    Eberly](documents/EberlyFISR.pdf), January 2002.
-   [Explanation by Chris Lomont with a newly computed \"magic\"
    constant](documents/LomontInvSqrt.pdf), February 2003
-   [Charles McEniry on the mathematics of the
    constant](documents/McEniryMathematicsBehind.pdf), August 2007
-   [Matthew Robertson\'s BS
    thesis](documents/RobertsonBriefHistory.pdf), April 2012
-   [Ben Self\'s BS
    thesis](documents/SelfEfficientComputation.pdf), May 2012
-   [Thomas Nelson\'s survey of the
    field](documents/NelsonSurvey.pdf), July 2017

### More recent works which situate the FISR

-   [Jerome Coonen\'s explainer with history of the
    FISR](documents/CoonenFunParts.pdf), April 2022
-   Noah Hellman\'s June 2020 [masters
    thesis](http://liu.diva-portal.org/smash/record.jsf?pid=diva2%3A1590166&dswid=-9978)
    demonstrating modern approximate computing on a logarithmic number
    system names the method used in the FISR as Mitchell\'s
-   My slide deck on the FISR, given at UW HCDE\'s autumn seminar series
    2022: [*What\'s in a
    Number?*](documents/FISR-Hyland-HCDENov2022.pdf)
-   [Mike Day\'s generalization of the Fast Reciprocal Square Root](https://arxiv.org/abs/2307.15600), posted in 2023, generalizes the FRSR to all rational powers. Day suggests we refer to it as the Fast Reciprocal Square Root, to avoid confusion with the term "inverse".
-   [Why does the integer representation of a floating point number offer a piecewise linear approximation to the logarithm?](https://stackoverflow.com/questions/75772363/why-does-the-integer-representation-of-a-floating-point-number-offer-a-piecewise) A 2023 Stack Overflow question which asks specifically about **why** the integer representation of a floating point number offers a piecewise linear approximation to the logarithm. The answers are quite good.

### The Wikipedia article

-   [Fast inverse square
    root](https://en.wikipedia.org/wiki/Fast_inverse_square_root). It is not up to date completely with all post-2010 changes in
    understanding. A source and place to find sources. (I wrote the
    first version but no longer maintain it.)

## The Fast Inverse Square Root before Quake III

> "If you only deal with positive numbers, the bit pattern of a floating point number, interpreted as an integer, gives a piecewise linear approximation to the logarithm function"

This quote, from Steve Gabriel and Gideon Yuval then at Microsoft, is passed along by Jim Blinn in his article "[Floating-Point Tricks](https://ieeexplore.ieee.org/abstract/document/595279)". Written in 1997, it is the first modern treatment of the scheme which the code in Quake III would make famous only a few years later.

### On Kahan and K-C Ng\'s 1986 version
The method which Cleve Moler cited in the creation of what became the FISR. This method was not formally published, but circulated among researchers and included as a comment in the [source code of the library "fdlibm"](https://www.netlib.org/fdlibm/e_sqrt.c), distributed via Netlib since 1993.
-   An
    [implementation](https://gist.github.com/Protonk/f3c5bb91f228ffec4d4c5e2eb16e489d)
    with explanatory comments in C
-   The unpublished [paper](https://adampunk.com/documents/softsqrt.pdf)
    from 1986
-   An [error analysis](https://apps.dtic.mil/sti/citations/ADA636844)
    of Kahan-Ng by Ren-Cang Li, one of Kahan\'s PhD students. This is
    the only paper I have found which cites the unpublished Kahan-Ng
    work.

## Square root approximations
The square root is a function which often appears in the critical path of a lot of applications and unlike addition, multiplication, and division, it is was not commonly implemented in hardware for most of the history of computing. As a result, many different software approximations have been developed.

### Jean-Michel Muller\'s excellent survey

-   Jean-Michel Muller, \"[Elementary Functions and Approximate
    Computing](https://doi.org/10.1109/JPROC.2020.2991885),\" in
    Proceedings of the IEEE, vol. 108, no. 12, pp. 2136-2149, Dec. 2020

### A "Reciproot" on the Manchester Mark I

-   One of the few remaining subroutines from the Manchester Mark I is a reciprocal square root. Martin Campbell-Kelly notes that in a manual written for the Mark I, "A total of ten sub-routines are named here; half were for input/output and half were mathematical functions." ([Programming the Mark I: Early programming activity at the University of Manchester](https://ieeexplore.ieee.org/document/4639134) [[Archive Link](https://archive.org/details/programming-the-mark-i)], p. 149) Kelly notes that the routine was first written in 1949. The extant copy [archived here](documents/ManchesterRecipRoot.pdf) is dated September 7, 1951.

## Updates to this page

This place is web 1.0 levels of under construction. If you feel this page missing something, please let me know. It is likely in my google drive awaiting migration here—if it is something **not** in my google drive, I would very much like to know about it.
