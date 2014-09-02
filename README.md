
# Aura Tutorials Website

This is the website for [rajaraodv.github.io/auratutorials](https://rajaraodv.github.io/auratutorials).
You can see the generated files at [https://github.com/rajaraodv/auratutorials](https://github.com/rajaraodv/auratutorials) repository.

# Submit Your Own Aura Tutorial
Built a cool Aura app? Simply fork this repo and submit a tutorial for that.

## Prerequisites
1.  Fork This Repo
		
2. Install [Hexo](http://hexo.io/)

``` bash
	  $ sudo npm install hexo -g
```
	
3. Install [Gulp](http://gulpjs.com/)
	
``` bash
		$ sudo npm install gulp -g
	
```
	
4. Clone the repo to your machine and run the server

``` bash
		$ git clone https://github.com/YOURREPO/auratutorials-site.git
		$ cd site
		$ npm install
```
		
5. Generate (Generates html files from markdown):
		
``` bash
		$ gulp
		$ hexo generate
```
	Note: You can use `hexo generate --watch` to auto generate when files are updated.
		
6. Run server:
		
``` bash
		$ hexo server
```
7. The server runs at: `http://localhost:4000`


## Add A Tutorial
Imagine you had a tutorial's name is `MyTutorial` and it has 2 files `tutorialFile1.md` and `tutorialFile2.md`

1. Open `_config.yml`
2. Add the following at the `doc_sidebar` section.

	```yaml	
		MyTutorial:
		   My Step 1: tutorialFile1.html
		   My Step 2: tutorialFile2.html
	```
	Note2:
	1. The above adds` MyTutorial`, `My Step 1` and `My Step 2` to the side bar.
	2. Use `.html`instead of `.md` in the _config files.
	3. **Yaml Errors:** `yaml` uses "spaces" extensively to create parent and child relationships. So make sure to use spaces properly. 
		For Example: If you use tabs or use an extra space, you will see an error.
	
3. Open `/source/tutorials/` and create your tutorials files like `tutorialFile1.md` and `tutorialFile2.md` there. 
4. Refresh browser to see changes.
5. If you have images, keep them in `/source/images/` folder and access them <img src="/images/myImageFile.png" />/ from your .md files.

## Submit a Pull Request
Once you are done with your tutorial, please send a pull request from your repo.


