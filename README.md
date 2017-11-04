# reviewBackbone
```
recently，i have time to review my previous cases;i decide to make some fundamental demos.
today i made a table by using backbone and my point is how to use backbone and change the coding logic .
And there are different ways to do it.here is mine.


### firstly, we should know how to construct mvc skeleton,especially view.

rules num 1 : the model containes Table fields:including age , hobby , job etc.

rules num 2 : the collection means data collections or model collections,And we need render into document.

relus num 3 : the view's job is rendering dcoument.


### avoid rendering whole page , we need Decomposing body,piece by piece

step 01 : construct a model ,we can defaults some. * maybe we need lots of model to render a part.
          var Trmodel = Backbone.Model.extend({
            defaults :{
                age :26,
                hobby : "codes",
                favorite : "coding"
            }
          })
          
          
step 02 : code a collection containes Trmodel
          var TrCollection = Backbone.Collection.extend({
                model:Trmodel
           }) 
           
step 03 : Decomposing Decomposing...
var TrrenderView = Backbone.View.extend({   // each tr how to code 
			tagName:"tr",
			render:function(){
				console.log( this.model )
				$(this.el).html(_.map([this.model.get('price'),this.model.get('quantity')],function (value,key) {
					return "<td>" + value + "</td>"
				}));
				return this;
			}
		})

		var TrListView = Backbone.View.extend({
			tagName:"table",
			render:function () {
				$(this.el).empty();
				//th
				$(this.el).append( $("<tr></tr>").html(_.map(['price','quantitu'],function(value){
					return "<td>"+ value +"</td>"
				}))  )
				//tr
				$(this.el).append(
					_.map(this.collection.models,function (model,key) {
						return new TrrenderView({
							model:model
						}).render().el
					})
				);
				return this;
			}
		})

		
		var wholePage = Backbone.View.extend({
			render:function () {
				$(this.el).html(new TrListView({
					collection:this.collection
				}).render().el)
			}
		})

		var jsonData = [
			{price:"1$",quantity:"1/signal"},
			{price:"2$",quantity:"1/signal"},
			{price:"3$",quantity:"1/signal"},
			{price:"4$",quantity:"1/signal"}
		]
		var TrCollections = new  TrCollection(jsonData);

		new wholePage({
			collection : TrCollections,
			el:"body"
		}).render()
    
    
    其实代码 内容 部分 都差不多，就是和大家 交流一下思想，顺便写几行英文。
    在我们用 mvc 或者说伪mvc 框架的时候，一定 搞清楚 这个框架的导向，backbone 其实并不是 真正意义上的mvc
    因为 她的 c 层 是  collection。字面上：就是集合，和mongodb的collection一个意思， 更像是 适配器，选择器，筛选器。
    并且backbone还有router 呈现页面。
    
    
    controller层 就是纽带？她的功能很关键，更像一个管理者，model负责数据，逻辑，view负责呈现，页面交互。
    
    
    在我们写代码的时候，数据层 和 视图层分离 ，通过 controller 来调度，依赖这个controller 层  举个例子：
    
    A kicks  B,  一般来讲 A 依赖 B ， 没有 B 这个对象  就没有完成这个动作，没打着阿。
    
    但是 现在 A 打 B ，A 不依赖 B ，而是 依赖 “kick” 这个动作，没有B ，无所谓，我可以打 C，反而更加灵活。
    
    而且 不会出现 ，一个 error 问题，导致 全部的代码瘫痪。
    
    最小知识原则，依赖反转等等 我们编码的时候经常听到的词，都和她有关。
    
   
  
    
    
    
    
    
    
    
    
