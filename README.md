##字号大小调整的技术方案调研

###类似的app实现效果

![QQ](http://7xjg07.com1.z0.glb.clouddn.com/fontE488A734-F0EF-4AC9-9D2A-C135CC3C8C27.png)
![wechat](http://7xjg07.com1.z0.glb.clouddn.com/fontDF78799D-95DC-4CD0-BEDE-8A7EC8AD2C0F.png)
![weibo](http://7xjg07.com1.z0.glb.clouddn.com/fontC476E237-33CF-47CD-8979-12AD0CF49536.png)

####说明
通过以上三组图片我们可以看到以下特点

-1.在顶部导航栏这里文字大小，图标大小并没有随着字体大小设置的改变而改变。

-2.底部导航栏中的图标和文字，除微信有做相应变化外，另外两个app均都没有发生明显变化

-3.凡是出现在中间部分的字体包括图标都发生了相应变化

####技术背景

Textkit是基于coreText的一个强大的文本渲染框架。现今所有与文本相关的组件都是基于Textkit。Dynamic Type是iOS7推出时随之而来的特性。它允许用户指定应用程序中文本字体大小和样式等。

#####Text Style
使用DynamicType这项API时我们不会是直接指定字体大小。我们需要font时。只要使用UIFont调用preferredFontForTextStyle:方法来实例化即可。
事实上一个textStyl只是一个预定义好的一个常亮，使用较为简单的方式来创建一个UIFontDescriptor或者UIFont。
 UIFont *myFont = [UIFont preferredFontForTextStyle:UIFontTextStyleHeadline];

		
	//目前iOS中有一下Style
	UIFontTextStyleTitle1 
	UIFontTextStyleTitle2 
	UIFontTextStyleTitle3 
	UIFontTextStyleCallout 
	UIFontTextStyleHeadline 
	UIFontTextStyleSubheadline 
	UIFontTextStyleBody 
	UIFontTextStyleFootnote 
	UIFontTextStyleCaption1 
	UIFontTextStyleCaption2 


####UIFontDescriptor 

UIFontDescriptor也是随着iOS7推出的，它用于描述字体的属性，比如字体、字号等。Font Descriptors一对属性的集合，我们可以通过仅改变descriptors中的一个属性来改变不同的字体。
比如一下代码

	UIFontDescriptor *bodyFontDesciptor = [UIFontDescriptor preferredFontDescriptorWithTextStyle:UIFontTextStyleBody];
	UIFontDescriptor *boldBodyFontDescriptor = [bodyFontDesciptor fontDescriptorWithSymbolicTraits:UIFontDescriptorTraitBold];
	self.boldBodyTextLabel.font = [UIFont fontWithDescriptor:boldBodyFontDescriptor size:0.0];
	
我们使用Text style来创建了一个对应的UIFontDescriptor实例。然后使用fontDescriptorWithSymbilicTraits:方法返回一个与上面的bodyFontDesciptor相同特点同时还具有指定特点的一个UIFontDescriptor实例。最后使用fontWithDescriptor:size:方法获得对应的字体。但是并不需要设置大小，所以返回size参数设置为0.0。那么返回的字体大小就取决于原先的设置。

####在app中修改字体样式的相关思路

1.设置界面中

![setting](http://7xjg07.com1.z0.glb.clouddn.com/fontA0B5C748-F487-4EC2-9BBB-5DB53BB7C4B0.png)

图中有7个档位，在iOS中分别代表7个Content size

	 UIContentSizeCategoryExtraSmall 
	 UIContentSizeCategorySmall 
	 UIContentSizeCategoryMedium 
	 UIContentSizeCategoryLarge 
	 UIContentSizeCategoryExtraLarge 
	 UIContentSizeCategoryExtraExtraLarge 
	 UIContentSizeCategoryExtraExtraExtraLarge 

每个Content size都对应有不同的style。每个style在都对应的font。
所以我们在修改字体大小的时候会基于两种情况：1.基于当前手机本身的默认字体大小进行修改 2.自己定义一个默认的初始值。两者的出发点都是一样的，要有一个初始值作为标准。之后我们就需要对这个默认值进行修改,我们可以通过设置修改量的多少来设定字体的大小





**参考文献**
	
－http://stackoverflow.com/questions/20510094/how-to-use-a-custom-font-with-dynamic-text-sizes-in-ios7 How to use a custom font with dynamic text sizes in iOS7

－http://www.priyaontech.com/2014/04/supporting-dynamic-text-in-your-ios-apps/ Supporting Dynamic Text in your iOS apps

－https://developer.apple.com/library/ios/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/CustomTextProcessing/CustomTextProcessing.html#//apple_ref/doc/uid/TP40009542-CH4-SW65  Using Text Kit to Draw and Manage Text	
	

	



	
