# VIM Plugins

## Taglist

Depends on ctags

### Install steps (ubuntu):

Install exuberant-ctags

	$ apt-get install exuberant-ctags

Download taglist from: <http://www.vim.org/scripts/script.php?script_id=273>

Setup taglist:

	$ cd ~/.vim; unzip .../taglist_46.zip
	$ cd doc; vim
	# run ':helptags .' in vim

### Config taglist:

Add some configs to vimrc:

	" Taglist settings
	let Tlist_Inc_Winwidth=0
	let Tlist_Use_Right_Window=1
	let Tlist_File_Fold_Auto_Close=1
 
Config options:

	Tlist_GainFocus_On_ToggleOpen : 为1则使用TlistToggle打开标签列表窗口后会获焦点至于标签列表窗口；为0则taglist打开后焦点仍保持在代码窗口
	Tlist_Auto_Open : 为1则Vim启动后自动打开标签列表窗口
	Tlist_Close_On_Select : 选择标签或文件后是否自动关闭标签列表窗口
	Tlist_Exit_OnlyWindow : Vim当前仅打开标签列表窗口时，是否自动退出Vim
	Tlist_Use_SingleClick : 是否将默认双击标答打开定义的方式更改为单击后打开标签
	Tlist_Auto_Highlight_Tag : 是否高亮显示当前标签。命令":TlistHighlightTag"也可达到同样效果
	Tlist_Highlight_Tag_On_BufEnter : 默认情况下，Vim打开/切换至一个新的缓冲区/文件后，标签列表窗口会自动将当前代码窗口对应的标签高亮显示。TlistHighlight_Tag_On_BufEnter置为0可禁止以上行为
	Tlist_Process_File_Always : 为1则即使标签列表窗口未打开，taglist仍然会在后台处理vim所打开文件的标签
	Tlist_Auto_Update : 打开/禁止taglist在打开新文件或修改文件后自动更新标签。禁止自动更新后，taglist仅在使用:TlistUpdate,:TlistAddFiles，或:TlistAddFilesRecursive命令后更新标签
	Tlist_File_Fold_Auto_Close : 自动关闭标签列表窗口中非激活文件/缓冲区所在文档标签树，仅显示当前缓冲区标签树
	Tlist_Sort_Type : 标签排序依据，可以为"name"（按标签名排序）或"order"（按标签在文件中出现的顺序，默认值）
	Tlist_Use_Horiz_Window : 标签列表窗口使用水平分割样式
	Tlist_Use_Right_Window : 标签列表窗口显示在右侧（使用垂直分割样式时）
	Tlist_WinWidth : 设定水平分割时标签列表窗口的宽度
	Tlist_WinHeight : 设定垂直分割时标签列表窗口的高度
	Tlist_Inc_Winwidth : 显示标签列表窗口时允许/禁止扩展Vim窗口宽度
	Tlist_Compact_Format : 减少标签列表窗口中的空白行
	Tlist_Enable_Fold_Column : 是否不显示Vim目录列
	Tlist_Display_Prototype : 是否在标签列表窗口用标签原型替代标签名
	Tlist_Display_Tag_Scope : 在标签名后是否显示标签有效范围
	Tlist_Show_Menu : 在图型界面Vim中，是否以下拉菜单方式显示当前文件中的标签
	Tlist_Max_Submenu_Item : 子菜单项上限值。如子菜单项超出此上限将会被分隔到多个子菜单中。缺省值为25
	Tlist_Max_Tag_Length : 标签菜单中标签长度上限

### Keys/usage:

	<CR> : 代码窗口跳转到标签列表窗口中光标所在标签定义处
	o : 在新建代码窗口中跳转到标签列表窗口中光标所在标签定义处
	P : 跳转至上一个窗口的标签处
	p : 代码窗口中内容跳转至标签定义处，光标保持在标签列表窗口中
	t : 在Vim新标签窗口中跳转至标签定义处。如文件已经在Vim标签窗口中打开，则跳转至此标签窗口
	Ctrl-t : 在Vim新标签窗口处跳转至标签定义处
	: 显示光标当前所在标签原型。对文件标签，显示文件的全路径名，文件类型和标签数量。对标签类型(指如variable/function等类别)，显示标签类型和拥有标签的数量;对函数/变量等普通标签，显示其定义的原型
	u : 更新标签列表窗口中的标签信息
	s : 切换标签排序类型(按名称序或出现顺序)
	d : 移除当前标签所在文件的所有标签
	x : 扩展/收缩标签列表窗口
	+ : 展开折叠节点*
	- : 折叠结点*
	* : 展开所有结点
	= : 折叠所有节点
	[[ : 跳转至上一个文件标签的头部
	<Backspace> : 跳转至上一个文件标签头部
	]] : 跳转至下一个文件标签头部
	<Tab> : 跳转至下一个文件标签头部
	q : 关闭标签列表窗口
	F1 : 显示帮助**
