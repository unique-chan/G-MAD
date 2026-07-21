<h3 align="center">
G-MAD: A Game-Based Data Generation Framework for Multi-View RGB-T Aerial Object Detection
</h3>

<p align="center">
  <a href="#"><img alt="Python3" src="https://img.shields.io/badge/Python-3-blue?logo=python&logoColor=white"></a>
  <a href="#"><img alt="Arma3" src="https://img.shields.io/badge/Game-Arma 3-red?logo=steam"></a>
  <a href="#"><img alt="Windows10-11" src="https://img.shields.io/badge/Platform-Windows_10_|_11-orange?logo=microsoft"></a>
  <a href="#"><img alt="MIT" src="https://img.shields.io/badge/License-MIT-green?logo=MIT"></a>
</p>

<hr>



### Preview
<p align="center">
    <img alt="Welcome" src="https://unique-chan.github.io/AMOD-Project/static/images/Homepage-fig1.jpg" />
</p>

### Updates
- (07/2026) 🎉 Our work has been accepted to ACM Multimedia (MM) 2026 (Main Proceeding / OSS Track)
- (06/2026) 📢 Using our G-MAD, we construct and release a new large-scale RGB-T multi-view benchmark dataset named **AMOD** for aerial object detection: Download our dataset [here](https://huggingface.co/datasets/unique-chan/AMOD)!
- (03/2026) Welcome!


### Quick start
- We assume that you have already finished setting up Arma 3 by our [tutorial](Tutorial.md) in advance.
- Please install necessary python libraries in `requirements.txt`
  ```bashshell
  conda create -n g-mad python=3.8 -y
  conda activate g-mad
  pip install -r .\requirements.txt
  ```
- For example, run the below command to generate **10** scenes of **sunny** day which covers all camera tilt cases between **-60** and **60** degrees, from **9AM** to **6PM**, on the map named **malden** for **training**:
	```bashshell
	python main.py  -weather 'sunny' -map_name 'malden' \
	                -arma_root_path 'C:/Users/{user_name}/Documents/Arma 3' \
	                -save_root_path 'C:/Users/{user_name}/Desktop' \
	                -start_hour 9 -end_hour 18 \
	                -n_times 10 -mode 'EO' \
	                -class_path 'classes/CLASSES.csv' \
	                -invalid_bbox_path 'classes/INVALID_BBOX.csv' \ 
	                -look_angle_min -60 -look_angle_max 60
	```
- To create only on-nadir view scenes, set both `look_angle_min` and `look_angle_max` to 0.
- ⭐ Try to use our GUI tool for convenience!
	```bashshell
	python main_GUI.py
	```

### Dataset structure
- The directory structure of our dataset is as follows:
~~~
|—— 📁 {train or test}_{map_name}_{weather}_{start_hour}_{end_hour}_...
	|—— 📁 0000 (scene number)
		|—— 📁 20  (look angle)
			|—— 🖼️ EO_0000_-0.png  
			|—— 📄 ANNOTATION-EO_0000_20.csv (including bbox labels)
		|—— 📁 +20 
			|—— 🖼️ ...
	|—— 📁 0001
		|—— 📁 -20
		|—— 📁 ... 
	|—— 📁 0002
		|—— 📁 -20
		|—— 📁 ...
	...
	|—— 📄 meta_..._.csv (including in-game shooting time, weather, and error logs per each scene)
~~~
- You may need to transform the above folder structure before training your own model.


### Citation
~~~
@inproceedings{G-MAD2026,
  title={G-MAD: A Game-Based Data Generation Framework for Multi-View RGB-T Aerial Object Detection},
  author={Kim, Yechan and Park, JongHyun and Yoon, Dongho and Jung, Namhoon and Jeon, Moongu},
  year={2026},
  booktitle={ACM International Conference on Multimedia}
}
~~~


### Contribution
- If you find any bugs for further improvements, please feel free to create issues on GitHub!
- All contributions and suggestions are welcome. Of course, stars (🌟) are always welcome.
