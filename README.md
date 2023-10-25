# HydroDL Website

This is the repo that stores the mkdocs yaml file to create our website. GitHub CI integration has been set up to deploy the website on a `git push`, or from a web-browser push to `main`

**NOTE:** When you are pushing to this project, please give a good commit name that you understand, then verify that the build worked in the `actions` panel of the repo. The CI push will have the same name as the commit that you wrote. 

## How to add Coding Project to the Code Panel:

1. Add your future page to the `mkdocs.yml` config file
- Open the `mkdocs.yml` file
- Scroll down to `nav:`
- Following the format on the page, add your page underneath the `- Code:` tab
  - It should look like this example below with `Your Title` being the name of your physics_model and `your_code.md` being the name of the file you will be creating in the next step.
``` yml
nav:
    .
    .
    - Code:
        - codes/index.md
        - Your Title: codes/your_code.md
```
2. Create Your Code's md page
- Go into the `docs/codes` folder and create the `your_code.md` file (Changing the name to whatever you used in the previous step)
- Follow the following template:
``` 
# Your Code title here

## Code Release


## Results


## Bibtex Citation

```
- **NOTE:** Latex is enabled. So if you want to use it, add a closing and leading $ like so: `$\delta$`
- **NOTE:** If your project is still in preprint, add an Admonition underneath the project title:
``` 
!!! info

    This paper is currently in preprint
```
- If you want to add a different Admonitions see [here](https://squidfunk.github.io/mkdocs-material/reference/admonitions/)

3. Add your project card to the `codes/index.md` page
  - Open the `codes/index.md` file
  - Add the following html/markdown to the page
``` 
<div class="result" markdown>
  <div class="grid cards" markdown>

-   [Your Title][your_code.md]

    ---
![Image Title](../assets/project-figures/YOUR_IMAGE.png){align="left" width="250"}
**INSERT DESCRIPTION**

  </div>
</div>
```
  - **NOTE:** If you want to add an image (as detailed above) add it to the `docs/assets/project-figures/` folder with a unique name, then link to it above where you see `YOUR_IMAGE.png`
  - Add your project link at the bottom of the page similar to the projects already there (under the last `</div>`)
```
  </div>
</div>

  [your_code.md]: ../codes/your_code.md
```
5. Verify your build works, and check mhpi.github.io to see your new project page.

See https://squidfunk.github.io/mkdocs-material/ for information on website customization.
