---
layout: "../layouts/Layout.astro"
title: "The Fast Inverse Square Root"
description: "A place for information about what became known as the Fast Inverse Square Root"
---

This place is web 1.0 levels of under construction. If you feel this
page missing something, please let me know. It is likely in my google
drive awaiting migration here\--if it is something \*\*not\*\* in my
google drive, I would very much like to know about it.

In 2001 an approximation of a reciprocal square root (y = 1/sqrt(x))
leaked to the internet, was attributed to a charismatic hacker, and
attained the kind of fame normally reserved for soap opera stars or
particularly photogenic zoo animals. This fame allows us to, quite
incidentally, follow the normally invisible history of a mathematical
approximation over time, across many organizations, and through
co-creation of formal knowledge.

Working with oral history interviews, archival material, and source
code, this project is an attempt to follow this trace which, far from
being born from the brow of a 90s programmer, spans most of the history
of the digital computer.

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

-   [Coonen\'s explainer with history of the
    FISR](documents/CoonenFunParts.pdf), April 2022
-   Noah Hellman\'s June 2020 [masters
    thesis](http://liu.diva-portal.org/smash/record.jsf?pid=diva2%3A1590166&dswid=-9978)
    demonstrating modern approximate computing on a logarithmic number
    system names the method used in the FISR as Mitchell\'s
-   My slide deck on the FISR, given at UW HCDE\'s autumn seminar series
    2022: [*What\'s in a
    Number?*](documents/FISR-Hyland-HCDENov2022.pdf)

### The Wikipedia article

-   [Fast inverse square
    root](https://en.wikipedia.org/wiki/Fast_inverse_square_root). It is
    not up to date completely with all post-2010 changes in
    understanding. A source and place to find sources. (I wrote the
    first version but no longer maintain it.)

### Jean-Michel Muller\'s excellent survey

-   Jean-Michel Muller, \"[Elementary Functions and Approximate
    Computing](https://doi.org/10.1109/JPROC.2020.2991885),\" in
    Proceedings of the IEEE, vol. 108, no. 12, pp. 2136-2149, Dec. 2020

## On Kahan and K-C Ng\'s 1986 version

-   An
    [implementation](https://gist.github.com/Protonk/f3c5bb91f228ffec4d4c5e2eb16e489d)
    with explanatory comments in C
-   The unpublished [paper](https://adampunk.com/documents/softsqrt.pdf)
    from 1986
-   An [error analysis](https://apps.dtic.mil/sti/citations/ADA636844)
    of Kahan-Ng by Ren-Cang Li, one of Kahan\'s PhD students. This is
    the only paper I have found which cites the unpublished Kahan-Ng
    work.