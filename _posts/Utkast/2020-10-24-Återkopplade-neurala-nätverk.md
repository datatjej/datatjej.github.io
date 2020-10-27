---
layout: post
title: 34. Återkopplande neurala nätverk
---

Återkopplande neurala nätverk\*(#fotnot1) -- **recurrent neural networks** (RNN) på engelska -- är en typ av neurala nätverk som tillåter oss att ta den **temporala aspekten** av språk i beaktning. Medan framåtriktade neurala nätverk måste ha all information tillgänglig från början och analyserar indatat inom ett givet spann, gör RNN det möjligt att hantera indatat inkrementellt, utan att veta längden på datat på förhand, som vid t.ex. taligenkänning.

Minimikravet för att ett nätverk ska räknas som ett RNN är att värdet hos någon enhet i nätverket är direkt eller indirekt **beroende av tidigare utdata** som indata ([Jurafsky & Martin 2019: kap 9: sida 2](https://web.stanford.edu/~jurafsky/slp3/9.pdf)). En av de enklaste typen av RNN är **Elman-nätverk** som består av... 

<font size="2"><a name="fotnot1">*</a> Eftersom hela MLT-programmet är på engelska är det ibland svårt att veta vad den bästa svenska översättningen av begrepp som "recurrent neural networks" egentligen är. Ofta googlar jag, och i det här fallet hittade jag 19 träffar för "återkoppla(n)de neurala nätverk" och 15 för "rekurrenta neurala nätverk". Ordet "rekurrent" ligger närmare engelskan men blir ganska svårbegripligt på svenska. Därför föredrar jag "återkopplande".</font><br>