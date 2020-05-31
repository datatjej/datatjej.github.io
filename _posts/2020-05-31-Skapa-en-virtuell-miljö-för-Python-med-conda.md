---
layout: post
title: 23. Skapa en virtuell miljö för Python med conda
---

Ibland kan det vara bra att skapa en **virtuell miljö** (eng. *virtual environment*) när man jobbar i olika Python-projekt. Det gör det nämligen möjligt att anpassa versionen av olika Python-moduler (t.ex. PyTorch) för enskilda projekt utan att det påverkar andra projekt. När man skapar en virtuell miljö skapar man en **individuellt namngiven kopia av Python** som håller koll på sina egna filer, mappar och path-variabler [[1](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/20/conda/)]. I den här steg-för-steg-guiden tänkte jag visa hur man sätter upp en virtuell miljö med hjälp av paket- och miljöhanteraren [conda](https://docs.conda.io/projects/conda/en/latest/index.html), som ursprungligen var en del av distributionssystemet [Anaconda](https://en.wikipedia.org/wiki/Anaconda_(Python_distribution)) innan det bröts ut och släpptes som ett eget paket. 

  **Steg 1)** Skapa och namnge miljön
  
  {% highlight bash %}
  conda create -n "namn_på_miljön" python==3.7
  {% endhighlight %}
  
  <p align="center">
  <img src="/images/create_environment.PNG" alt="Lista över moduler som kommer att installeras för miljön" border="10" /> <br>
  En lista över moduler som kommar att installeras för miljön. Skriv in "y" för att gå vidare.
  </p>
  
  **Steg 2)** Tryck y + enter för att gå vidare
  {% highlight bash %}
    y
  {% endhighlight %}
  
  **Steg 3)** Aktivera miljön
  {% highlight bash %}
  conda activate namn_på_miljön
  {% endhighlight %}
  
  <p align="center">
  <img src="/images/activate_environment.PNG" alt="Aktivering av den virtuella miljön" border="10" /> <br>
  Observera övergången från (base) till den virtuella miljön (i det här fallet namngiven som "pythorchEnv") efter att den virtuella miljön aktiverats.
  </p>
  
  **Steg 4)** Installera ytterligare moduler du vill ha, exempelvis:
  {% highlight bash %}
  conda install pytorch torchvision cpuonly -c pytorch #För att installera PyTorch
  python3 -m pip install jupyter
  python3 -m pip install ipykernel
  python3 -m ipykernel install --user --name namn_på_miljön --display-name "namn_på_miljön"
  python3 -m pip install pandas
  python3 -m pip install matplotlib
  
  **Obs:** *De tre översta raderna, för jupyter och ipykernel, behövs för att använda miljön i Jupyter Notebook!* 
