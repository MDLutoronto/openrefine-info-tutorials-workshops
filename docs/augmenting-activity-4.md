---
title: "OpenRefine Augmenting Activity 4: Using Python"
parent: Intermediate Tutorials
grand_parent: "OpenRefine: Information, Tutorials, and Workshops"
layout: "home"
staff:
    - name: Kelly Schultz
      link: https://library.utoronto.ca/staff/kelly-schultz
maintainer:
    - name: Nick Field
      link: https://library.utoronto.ca/staff/nick-field
created_date: 2019-04-08
nav_order: 4.4
---
# OpenRefine Augmenting Activity 4: Using Python

The goal of this activity is to try out running Python in OpenRefine. We will use it to make an API call to augment our books dataset with information on the authors from Wikipedia.

***Note:** Complete Augmenting Activities [1](https://mdlutoronto.github.io/openrefine-info-tutorials-workshops/augmenting-activity-1/), [2](https://mdlutoronto.github.io/openrefine-info-tutorials-workshops/augmenting-activity-2/) & [3](https://mdlutoronto.github.io/openrefine-info-tutorials-workshops/augmenting-activity-3/) first before attempting this activity.*

*This tutorial has been developed for OpenRefine version 3\.7\.5*

Sometimes when you construct an API call and use the Add Column by Fetching URLs feature, it won’t work. In these cases, you can use python to help. So far we’ve been writing GREL expressions to create values to populate new columns, but we can also run python scripts to do that, especially when what we need to do is a bit more complicated or harder to do using GREL.

***Note on Python:** There are many ways to set up python on your computer. This activity assumes that you have Anaconda (**<https://www.anaconda.com/download>**) installed and that you know the path on your computer to where your additional python libraries are installed. This varies, even with Anaconda installed. I will be using the path that works on my computer, but this might be different for yours. Contact [mdl@library.utoronto.ca](mailto:mdl@library.utoronto.ca) if you need help with this setup.*

Augmenting Data using Python
----------------------------

1. Wikimedia provides a web API that we can use to access Wikipedia data. To learn more about how to use this API, see: <https://en.wikipedia.org/api/rest_v1/>

2. This page helps you construct URLs to make specific API calls. You can expand the section you are interested in, fill in the form and click on the **Try it Out** button to see what results you will get from a call. For our example, we are going to use the summary page example. On the Wikimedia REST API documentation page, first expand **Page Content.** 

    ![REST API menu]({{ '/assets/images/openrefine_REST01.png' | relative_url }})

    Next, find **/page/summary.**

    ![REST API /page/summary/ command]({{ '/assets/images/openrefine_REST02.png' | relative_url }})

    This instructs us that the URL is formed by **"<https://en.wikipedia.org/api/rest_v1/page/summary>" \+ value**, where value is the title of the page. If we google “wikipedia jane austen”, we will see that the URL normally has a title formed by the first name, then last name, with all spaces replaced by underscores.  

    ![The beginning of the Wikipedia article on Jane Austen, with the URL ending in Jane_Austen]({{ '/assets/images/openrefineworkshop115.png' | relative_url }})

3. So first we need our author names in that format to make this call – which we already partially did in [Augmenting Activity 1](https://mdlutoronto.github.io/openrefine-info-tutorials-workshops/augmenting-activity-1/). We reversed the names, but we have not replaced the spaces with underscores yet. From the **Full Author Name** column pull down menu, select **edit cells \> transform**. To use the GREL replace function to substitute spaces with underscores, type:

    ```
    value.replace(" ", "_")
    ```
    If the preview looks correct, click on **OK** to apply the transformation.

    ![Transformation window, showing Jane Austen in the input and Jane_Austen in the output.]({{ '/assets/images/openrefineworkshop116a.png' | relative_url }})

4. Now that we have our author names in the correct format, we are ready to make our python call. You might be wondering why we can’t just use the Add Column by Fetching URLs feature in OpenRefine. If you want, **give it a try and see what happens**. You should see a column of blanks. The API works, but it is doing things that OpenRefine doesn’t expect. But we have another option; we can use python to make the API call.

5. To use python in this way, we have to make sure we have the **urllib3** python library installed, as this library allows us to make API calls.

    ***Note:** A python library is just additional code that can help you extend what you can do with python. When you install Anaconda, it comes with various common python libraries installed. But it is common practice to install other libraries to augment what you can do with python, so Anaconda also allows you to install additional libraries.*

    To check which libraries are installed, just start up **Anaconda Navigator** and go to the **Environments section**. There you can see what libraries are installed and search for new libraries to install.  

    ![In Anaconda, go to the Environments section, from the drop down menu select Installed, then check if urllib3 is installed.]({{ '/assets/images/anaconda.png' | relative_url }})

    ***Note:** If urllib3 is not installed, first, change the dropdown menu from “Installed” to “All.” Second, search for urllib3\. Third, once it shows up in the list, select the check box next to its name. Fourth, click Apply at the bottom of the window.*

    ![If urllib3 is not installed, 1. change the drop down menu to view all packages (not just installed), 2. search for urllib3, 3. once urllib3 appears as a downloadable library, check it, then 4. click apply.]({{ '/assets/images/anaconda1b.png' | relative_url }})

    *A window will pop up with a list of packages in or required for urllib3\. Click Apply again to confirm.*

    ![You will receive a notice that 6 packages will be installed. Click apply.]({{ '/assets/images/anaconda2a.png' | relative_url }})

6. After we install urllib3, we need to know where it is installed on our computer (i.e., what is the path to the folder). This is going to vary by how python and even Anaconda is set up. In the Map \& Data Library computer lab, the file pathway is **C:\\Program Files (x86\)\\Esri\\Data Interoperability (x86\)\\python**

    ***Note:** One way to do this, is to click on the play button next to “root” from the Environments section in Anaconda Navigator, and select “Open Terminal”. You should see a path to where Anaconda is installed, which can help you get started looking for where the urllib3 folder is. Try looking for folders like “Lib” or “library” from where it is installed. In there, try looking for a folder called “site\-packages”. This is where my personal urllib3 folder is, but yours might be somewhere else. Feel free to contact [mdl@library.utoronto.ca](mailto:mdl@library.utoronto.ca) for help with this.*

    ![In Anaconda, select Environments, click on the play button next to root, then select Open Terminal.]({{ '/assets/images/anaconda3.png' | relative_url }})

7. Now we are ready to create a new column and populate it with data from our API call. From the **Full Author Name** column pull down menu, select **edit columns \> add a column based on this column** and give it the name **Wikipedia**.

    <img src='{{ '/assets/images/OpenRefine8_7.png' | relative_url }}' alt='' title='' width='50%' height='625' />

8. You should notice there is a pull down menu that currently shows GREL as our expression language. **Pull down and select Python/Jython instead**.

    <img src='{{ '/assets/images/OpenRefine8_8.png' | relative_url }}' alt='' title='' width='90%' height='768' />

    For the expression, type:

    ```
    import sys
    sys.path.append(r'C:\Program Files (x86)\Esri\Data Interoperability (x86)\python')
    import urllib3
    http = urllib3.PoolManager()
    url = "https://en.wikipedia.org/api/rest_v1/page/summary/" + value
    r = http.request('GET', url)
    if r.status == 200:
      return r.data
    else:
      return r.status
    ```
    <img src='{{ '/assets/images/OpenRefine8_8b.png' | relative_url }}' alt='' title='' width='90%' height='975' />

9. This python code tells OpenRefine where my python libraries are installed on my computer. You would **modify the sys.path.append** line to point to where your urllib3 library is installed on your computer. Then you tell it that you want to use the urllib3 library.

    *More info on this can be found here: [https://github.com/OpenRefine/OpenRefine/wiki/Extending\-Jython\-with\-pypi\-modules](https://github.com/OpenRefine/OpenRefine/wiki/Extending-Jython-with-pypi-modules)*

    For the python code to make the API call, I used this library’s documentation here: [https://urllib3\.readthedocs.io/en/latest/](https://urllib3.readthedocs.io/en/latest/).

    For this tutorial, the URL for the API that we determined earlier: **"<https://en.wikipedia.org/api/rest_v1/page/summary/>" \+ value** 

    I also added an "if" statement at the end to tell it to return the data if all is well (i.e., getting the status code equal to 200\) or return the status code to see if something went wrong.

    ***Note:** Make sure that you indent the two return statements. Also, if you see “internal error” just wait a few seconds, as there is a delay. You should see the results pop up in the preview window.*

10. Click on **OK.**

11. You should see that the call worked and the JSON data is in my new **wikipedia** column. Finally, I need to create a new column that pulls out the summary extract about the author. From the **wikipedia** column pull down menu, select **edit columns \> add a column based on this column** and give it the name **Author Bio**.

12. Change the language back to **GREL**. For the expression, type:

    ```
    value.parseJson()["extract"]
    ```
    We can see from the preview that we have grabbed the part of the JSON data we are looking for. Finally, click on **OK.** 

    ![Create a new column, change language back to GREL, write or paste your code, and click OK.]({{ '/assets/images/anaconda5a.png' | relative_url }})

    You should see that our dataset is now augmented with a new column with information about each author in our dataset.

    ![The Author Bio column now contains the information from Wikipedia.]({{ '/assets/images/anaconda5b.png' | relative_url }})

And that's it! You've now successfully augmented the original dataset with data from Wikidata and Wikipedia, using OpenRefine's reconciliation service and python.

**Technique:** [Cleaning data](https://mdlutoronto.github.io/tutorials-search/?technique=Cleaning+data), [Extracting data](https://mdlutoronto.github.io/tutorials-search/?technique=Extracting+data) \| **Tools:** [OpenRefine](https://mdlutoronto.github.io/tutorials-search/?tool=OpenRefine), [Python](https://mdlutoronto.github.io/tutorials-search/?tool=Python)  
