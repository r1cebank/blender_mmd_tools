mmd_tools
===========

About
----
mmd_tools is a blender import addon for importing MMD (MikuMikuDance) model data (.pmd, .pmx) and motion data (.vmd)


### Environment

#### Compatible Version
blender 2.67 and later

#### Tested working environments
Windows 7 + blender 2.67 64bit

OSX + blender 2.67 64bit

Ubuntu + blender 2.67 64bit


Usage
---------
### Download

* download mmd_tools from our github repository.
    * https://github.com/sugiany/blender_mmd_tools
* Stable release can be found in the link below.
    * [Tags](https://github.com/sugiany/blender_mmd_tools/tags)
* Nightly release can be downloaded from the master, only basic functionality is tested working.
    * [master.zip](https://github.com/sugiany/blender_mmd_tools/archive/master.zip)

### Install
Extract the archive and put the folder mmd__tools into the addon folder of blender.

    .../blender-2.67-windows64/2.67/scripts/addons/

### Loading Addon
1. In User Preferences, under addon tab, select User filter and click the checkbox before mmd_tools
   (you can also find the addon my search)
2. After installation, you can find the panel called MMD on the left of 3D view

### Importing MMD Models
1. Please select "import/Model" button in the mmd_tools panel
2. A file selection window will open and please select the model file you want to import

### Importing MMD Motion Data
1. According to the motion type you want to import, please select Mesh, Armature or Camera from the project window. (Nothing will be imported if nothing is selected)
2. Click on the "import/Motion" button from the mmd_blender panel.
3. A file selection window will open and please select the motion file you want to import
4. Leave「update scene settings」checked will automatically update the frame range and scene after motion is imported. 


Additional Information
-------------------------------
### Import Model
This will import the MMD model data, compatible format are up to pmx(ver2.0). 
We suggest that you leave the options as default.
If regid body is not loading properly, check "import only non dynamics rigid bodies".

* scale
    * If you ever change the scale od the model, please set the same scale for all your motions.
* rename bones
    * Automatically rename bone name into blender-friendly names（Ex. 右腕→腕.L）
* hide rigid bodies and joints
    * Hide all objects with regid body information.
* import only non dynamics rigid bodies
    * When using with cloth or soft body, which regid information should be ignored, please check this option.
* ignore non collision groups
    * Ignore loading of all non collision groups, when loading model and freeze during animation, please select this.
* distance of ignore collisions
    * Ignore distance for collisions groups.
* use MIP map for UV textures
    * Switches Blender's auto MIP map generator.
    * Please switch off when violet noise showing up for textures with alpha channels.
* influence of .sph textures
    * Set the intensity of sphere map (0.0～1.0)
* influence of .spa textures
    * Set the intensity of sphere map (0.0～1.0)

### Import Motion
Import Motion to the currently selected Armature, Mesh or Camera from vmd.

* scale
    * This is scale, please match this with the scale you used for Importing Model.
* margin
    * Blank frames for physics simulations.
    * When the initial motion is too far from the origin,  physics engine will give incorrect simulation when the motion is started.
    To avoid this, please set the margin to some value, it will insert some blank frames into the beginning of the timeline to avoid this problem.
* update scene settings
    * After motion data is loaded, it will automatically update the scene for you including the scene and the frame range in animation timeline.
    * Frame range is needed to correctly render all the frames in the scene.
    * This will also change the framerate to 30fps

### Set frame range
Change the current frame range to the frame range of the current animation loaded in the scene.
This also change the framerate into 30fps.

* This feature is the same as the "update scene settings" in Import vmd function.

### View

#### GLSL
Automatically set the current shading mode into GLSL

* Shading mode will be changed into GLSL.
* Change "shadeless" property of all the materials in the scene into off.
* Add a Hemi light.
* When button is clicked, 3D view's viewport shading mode will be changed into "Textured"

#### Shadeless
Automatically set the current shading mode into Shadeless

* Shading mode will be changed into GLSL.
* Change "shadeless" property of all materials in the scene into on.
* When button is clicked, 3D view's viewport shading mode will be changed into "Textured"

#### Cycles
Automatically change all the materials in the scene into Cycles materials.

* 何の根拠もない適当な変換です。
* 完了メッセージなどは表示されません。マテリアルパネルから変換されているかどうか確認してください。
* ボタンを押した3DViewのシェーディングをMaterialに変更します。
    * 3DViewのシェーディングをRenderedに変更すれば、Cyclesのリアルタイムプレビューが可能です。
* ライティングは変更しません。設定が面倒な場合は、WorldのColorを白(1,1,1)に変更すればそれなりに見えます。

#### Reset
Reset the model and its materials back to the initial state.


#### Separate by materials
選択したメッシュオブジェクトのメッシュをマテリアル毎に分割し、分割後のオブジェクト名を各マテリアル名に変更します。
* blenderデフォルトの"Separate"→"By Material"機能を使用しています。


Others
------
* カメラとキャラクタモーションが別ファイルの場合は、ArmatureとMeshを選択してキャラモーション、Cameraを選択してカメラモーションというように2回に分けてインポートしてください。
* モーションデータのインポート時はボーン名を利用して各ボーンにモーションを適用します。
    * ボーン名と構造がMMDモデルと一致していれば、オリジナルのモデル等にもモーションのインポートが可能です。
    * mmd_tools以外の方法によってMMDモデルを読み込む場合、ボーン名をMMDモデルと一致させてください。
* カメラはMMD_Cameraという名前のEmptyオブジェクトを生成し、このオブジェクトにモーションをアサインします。
* 複数のモーションをインポートする場合やフレームにオフセットをつけてインポートしたい場合は、NLAエディタでアニメーションを編集してください。
* アニメーションの初期位置がモデルの原点と大きく離れている場合、剛体シミュレーションが破綻することがあります。その際は、vmdインポートパラメータ"margin"を大きくしてください。
* モーションデータは物理シミュレーションの破綻を防止するため"余白"が追加されます。この余白はvmdインポート時に指定する"margin"の値です。
    * インポートしたモーション本体は"margin"の値+1フレーム目から開始されます。（例：margin=5の場合、6フレーム目がvmdモーションの0フレーム目になります）
* pmxインポートについて
    * 頂点のウェイト情報がSDEFの場合、BDEF2と同じ扱いを行います。
    * 頂点モーフ以外のモーフ情報には対応していません。
    * 剛体設定の"物理+ボーン位置合わせ"は"物理演算"として扱います。
* 複数のpmxファイルをインポートする場合はscaleを統一してください。


Known issues
----------
* 剛体の非衝突グループを強引に解決しているため、剛体の数が多いモデルを読み込むとフリーズすることがあります。
    * 正確には完全なフリーズではなく、読み込みに異常な時間がかかっているだけです。
    * フリーズするモデルを読み込む場合は、"ignore non collision groups"オプションにチェックを入れてください。
    * 上記オプションをオンにした場合、意図しない剛体同士が干渉し、正常に物理シミュレーションが動作しない可能性があります。
* 「移動付与」ボーンは正常に動作しません。
* オブジェクトの座標（rootのemptyおよびArmature）を原点から移動させると、ボーン構造が破綻することがあります。
    * モデルを移動させたい場合は、オブジェクトモードでの移動は行わず、Pose Modeで「センター」や「全ての親」などのボーンを移動させてください。
    * 現状、解決が難しいため、オブジェクトモードでの移動操作は行わないことをおすすめします。


Bug・Request・Questions etc.
------------------
Please submit a GitHub issue or contact me using twitter 
[@sugiany](https://twitter.com/sugiany)

English README translated by [r1cebank](https://github.com/r1cebank)


Changelog
--------
Please refer to CHANGELOG.md


License
----------
&copy; 2012-2014 sugiany  
Distributed under the MIT License.  
