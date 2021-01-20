---
layout: post
title: 23. Skapa en virtuell miljö för Python med conda
---

Ibland kan det vara bra att skapa en **virtuell miljö** (eng. *virtual environment*) när man jobbar i olika Python-projekt. Det gör det nämligen möjligt att anpassa versionen av olika Python-moduler (t.ex. PyTorch) för enskilda projekt utan att det påverkar andra projekt. När man skapar en virtuell miljö skapar man en **individuellt namngiven kopia av Python** som håller koll på sina egna filer, mappar och path-variabler [[1](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/20/conda/)]. 

I den här steg-för-steg-guiden tänkte jag visa hur man sätter upp en virtuell miljö med hjälp av paket- och miljöhanteraren [conda](https://docs.conda.io/projects/conda/en/latest/index.html), som ursprungligen var en del av distributionssystemet [Anaconda](https://en.wikipedia.org/wiki/Anaconda_(Python_distribution)) innan det bröts ut och släpptes som ett eget paket. 

 <h2>Skapa en virtuell miljö</h2>
 
 Börja med att öppna terminalen. Jag använder [Ubuntu-terminalen](https://ubuntu.com/tutorials/tutorial-ubuntu-on-windows#1-overview) för Windows 10. 

  **Steg 1) Skapa och namnge miljön:**
  
  {% highlight bash %}
  conda create -n namn_på_miljön python=3.7
  {% endhighlight %}
  
  <p align="center">
  <img src="/images/create_environment.PNG" alt="Lista över moduler som kommer att installeras för miljön" border="10" /> <br>
  En lista över moduler som kommar att installeras för miljön.
  </p>
  
  **Steg 2) Tryck ja för att gå vidare:**
  {% highlight bash %}
  y + enter
  {% endhighlight %}
  
  **Steg 3) Aktivera miljön:**
  {% highlight bash %}
  conda activate namn_på_miljön
  {% endhighlight %}
  
  <p align="center">
  <img src="/images/activate_environment.PNG" alt="Aktivering av den virtuella miljön" border="10" /> <br>
  Observera övergången från (base) till den virtuella miljön (i det här fallet namngiven som "pytorchEnv") efter att den virtuella miljön aktiverats.
  </p>
  
  **Steg 4) Installera ytterligare moduler du vill ha, exempelvis:**
  {% highlight bash %}
  python3 -m pip install jupyter
  python3 -m pip install ipykernel
  python3 -m ipykernel install --user --name namn_på_miljön --display-name "namn_på_miljön"
  python3 -m pip install matplotlib
  conda install pandas
  {% endhighlight %}
  
  **Obs:** *De tre översta raderna, för jupyter och ipykernel, behövs för att använda miljön i Jupyter Notebook! För PyTorch-installation, gå till [PyTorch-hemsidan](https://pytorch.org/get-started/locally/) för att se vilken version du bör installera.* 
  
  <h2>Om du vill använda den virtuella miljön i Jupyter Notebook:</h2>
  
  **Steg 5) Gå till rätt mapp på datorn och skriv in:**
  {% highlight bash %}
   jupyter notebook
  {% endhighlight %}

  **Steg 6) Klistra in någon av URL-koderna i webbläsaren:**
  
  <img src="/images/jupyter_notebook_url.PNG" alt="Länkar genererade för Jupyter Notebook-filer" border="10" /> <br>
  
  **Steg 7) Byt ut miljön i kärnan:**
  
  <img src="/images/change_kernel.PNG" alt="Byt miljö i kärnan" border="10" /> <br>
