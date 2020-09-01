---
layout: post
title: 26. Synka en fork (GitHub)
---

Det finns två sätt att synka en fork så att den uppdateras med de ändringar som gjorts i den ursprungliga repon efter forkningen.

# Synka i terminalen 

1. Öppna Git Bash på forkmappen.

***********KONFIGURERA REMOTE-REPO*****************************
*Börja med att konfigurera en remote-repo som pekar på den ursprungliga repon enligt följande:*

2. Lista de nuvarande konfigurerade remote-reporna för forken:

{% highlight git %}
git remote -v
origin  https://github.com/datatjej/comp-syntax-2020.git (fetch)
origin  https://github.com/datatjej/comp-syntax-2020.git (push)
{% endhighlight %}

***********SLUT PÅ KONFIGURERA REMOTE-REPO***********************

