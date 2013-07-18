---
title: 对象属性
---

.. contents::
.. sectnum::


对象属性
==============================================

对象的属性，通过接口分类如下

流程单的属性
--------------------------------------

得到流程单的表单自定义字段的值

IFieldStorage(context)['field_name']

如果context是表单，那更简单的写法是：

context['field_name']

设置信息
--------------------------------------

包括流程和扩展应用的设置，可采用如下方法得到：

ISettings(context)['location']

当然这里的context，应该是流程容器或者应用，如果是在流程中取设置，即是container

都柏林核心元数据
--------------------------------------

系统的所有对象，都包括一组标准的元数据，也就是所谓的都柏林核心元数据（这是一个图书馆元数据国际标准）

- IDublinCore(obj).title 对象的标题

- IDublinCore(obj).description 对象的描述信息

- IDublinCore(obj).identifier 这个也就是文件的编号

  注意：文件的编号默认和对象的永久标识是相同的，但是编号是可以自由调整的

- IDublinCore(obj).creators 对象的创建人

  注意，这是个list类型的对象

- IDublinCore(obj).created 对象的创建时间

- IDublinCore(obj).modified 对象的修改时间

- IDublinCore(obj).expires 对象的失效时间

- IDublinCore(obj).effective 对象的生效时间

扩展属性
--------------
获取文件、文件夹的自定义扩展属性的值::

 IExtendedMetadata(context).getMetadata(‘contact’)[‘money’]

例子中的contact就是一个扩展属性，money是contact中的一个字段

另外，如果需要在软件包的安装程序中安装一个扩展属性，可以使用::

  metadata_definitions.installFromPackage('zopen.upload_control.upload_control',
      new_name='upload_control', 
      apply_to=['Folder'])

例子里zopen.upload_control.upload_control是zopen.upload_control这个软件包中名字为upload_control的表单。

apply_to这个list里面可以有的值及对应解释如下::

  'Folder'				文件夹
  'I3d'					3D文件
  'WorkOnlineFolder'		        扩展设置
  'WorkDesk'			        个人设置
  'File'				文件
  'Image'				图片
  'Document'			        文档


PersistentList，PersistentDict
================================================
Python的基础类型包括list和dict，都是mutable的，这2种对象发生变更修改，不会字段通知ZODB更新数据库。这样导致数据修改不能保存。

为了解决这一问题，对于需要保存的数据库中的数据，应该采用PersistentList来代替list，使用PersistentDict代替dict。他们的使用接口其实是一致的。
