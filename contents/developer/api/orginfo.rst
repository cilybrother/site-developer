---
title: 用户管理
description: 用户、组织结构和团队访问接口org_info
---

==============
用户管理
==============

.. contents::
.. sectnum::

用户管理
===============================

org_info API
------------------

查找相关
................

getPrincipalInfo(pid,skip_cache=False):
     得到实体的基本信息。实体：用户、组(部门、群组、角色)等

     如果是用户，返回::

             {'id':id, 'title': titlei, 'mobile': mobile, 'phone':phone, 'location':location 'email': email, 'parent': , 'disable':}

     如果是组(部门、群组、角色),返回::

             {'id':id, 'title': title, 'parent':}

hasUser(username):
   是否存在username,没有users.开头

listPrincipalInfo(pids,skip_cache=False):
     得到一组实体的基本信息。实体：用户、组(部门、群组、角色)等

     如果是用户，返回::

             [{'id':id, 'title': titlei, 'mobile': mobile, 'email': email, 'parent': , 'disable':}]

     如果是组(部门、群组、角色),返回::

             [{'id':id, 'title': title, 'parent':}]   

listUserGroups(user_id):
     列出人员所属的组(部门、群组、角色、岗位)信息。
     返回：::

           {'ous':[ou_id, ...],
            'groups':[group_id, ...],
            'roles':[role_id, ...],
            'licenses':[service_name, ...]
            }

listFlatUserGroups(user_id):
     通过人员id找到所属的全部组，这个用于检查权限。

listGroupMembers(group_id):
     列出组(部门、群组、角色、岗位)的人员清单，子部门、子组里面包含的用户，都会返回。这个用于发信。返回：::

           [member_id, ...]

getOUDetail(ou_id, include_disabled=False, skip_cache=False):
     部门详细信息，直接包含人员清单、组、子部门详细信息。
     返回：::

          {  'id':
             'title':
             'parent':
             'users':[id]
             'groups':[id] 
             'ous':[id]
          }

listOrgStructure(hide_team=False):
 得到组织架构信息，包括组织架构中的组(部门、群组、岗位)信息::

  {'groups': [],
  'id': u'groups.tree.default',
  'jobs': [{'id': u'groups.jobs.791112', 'title': u'xcvx'}],
  'ous': [{'groups': [],
          'id': u'groups.tree.468511',
          'jobs': [{'id': u'groups.jobs.570410', 'title': u'\u603b\u88c1'},
                   {'id': u'groups.jobs.519669',
                    'title': u'\u526f\u603b\u88c1'},
                   {'id': u'groups.jobs.304170',
                    'title': u'\u603b\u7ecf\u7406'},
                   {'id': u'groups.jobs.214984',
                    'title': u'\u603b\u7ecf\u7406\u52a9\u7406'},
                   {'id': u'groups.jobs.436675', 'title': u''}],
          'ous': [{'groups': [],
                   'id': u'groups.tree.110040',
                   'jobs': [],
                   'ous': [],
                   'title': u'rfedf',
                   'type': 'department'},
                  {'groups': [],
                   'id': u'groups.tree.471172',
                   'jobs': [],
                   'ous': [],
                   'title': u'fdf',
                   'type': 'department'},
                  {'groups': [],
                   'id': u'groups.tree.419478',
                   'jobs': [],
                   'ous': [],
                   'title': u'fd',
                   'type': 'department'},
                  {'groups': [],
                   'id': u'groups.tree.689057',
                   'jobs': [],
                   'ous': [],
                   'title': u'fdsfdsfdsf',
                   'type': 'department'}],
          'title': u'\u603b\u88c1\u529e',
          'type': 'department'},
         {'groups': [],
          'id': u'groups.tree.641936',
          'jobs': [{'id': u'groups.jobs.701987',
                    'title': u'\u884c\u653f\u7ecf\u7406'},
                   {'id': u'groups.jobs.839766',
                    'title': u'\u884c\u653f\u6587\u5458'},
                   {'id': u'groups.jobs.552599', 'title': u'\u524d\u53f0'},
                   {'id': u'groups.jobs.562020', 'title': u''}],
          'ous': [],
          'title': u'\u884c\u653f\u90e8',
          'type': 'department'},
         {'groups': [],
          'id': u'groups.tree.groups.tree.641936',
          'jobs': [{'id': u'groups.jobs.groups.tree.231151',
                    'title': u'\u4eba\u529b\u8d44\u6e90\u90e8'},
                   {'id': u'groups.jobs.578591', 'title': u''}],
          'ous': [],
          'title': u'\u884c\u653f\u90e8',
          'type': 'department'}],
  'title': u'\u5e7f\u5dde\u6613\u5ea6',
  'type': 'company'}


公司和实例信息
....................
listCompanies():
  得到(客户)公司的清单，返回::

    [{'id':id, 'title':title}]

listInstances(skip_cache=False):
 得到全部的站点实例::

   {
     instance_name:{'url':url, 'title':title},
      ...
   }

审核(上下级)相关
................
lookupReviewer( user_id, reviewer_table, step=''):
     查找审核人，通过审核人表来查找，主要在流程中使用

同步接口
.................
同步主要用于多用户数据源之间的同步，主要涉及 新增和编辑，不会有删除操作。

sync(ous=None, groups=None, users=None, send_mail=True, async=False)
   参数::

        ous: [{'id':, title:, parent:}]
        groups: [{'id':'', title:,  parent:'', members:[]}]
        users: [{'id':, 'title':, 'email':, 'mobile':, 'phone':, 'location':, 'parent':, ldap_username': 'disable': 'password':},]
        send_mail: 当新建人员的时候，默认发送邮件
        async： 这个接口默认是同步执行，当这个参数为真，以异步执行

   - 如果name不传递，则会新建一个。
   - name、parent 不需要加入users. 或groups.tree.
   - 如果title等属性不传入，则不修改该属性
   - parent 值都是部门id
   - ous 不传parent， 创建的是外部公司

管理接口
.............
removeOUs( ous_ids):
   删除一组部门

removeGroups( groups_ids):
   删除一组部门

removeUser( name)
   删除一个用户

set_allowed_services(username, app_name, instance_name, services)
   给用户分配许可::

      services: [service_name, service_name ...] # docs/projects/depts

get_allowed_services(username, app_name, instance_name)
   取得用户分配许可, 返回 ::

      [service_name, service_name ...]

set_ldap_config(server_address, enable=True)
   设置ldap配置信息

get_ldap_config()
   设置ldap配置信息

remove_group_users(group_id)
   从组里面移除一组人员

add_group_users(group_id, user_ids):
   添加一组人员到组里面

得到当前站点实例
----------------------
::

  full_instance_name = getName(getRoot()) # default.zopen.test
  instance_name = full_instance_name.split('.', 1)[0]


用户和组的内部ID
------------------

系统的用户ID皆为字符串类型，xxx为用户在系统中的登录名，下文中用户ID将用uesr_id来代替。

- 'zope.anyone'：匿名用户
- 'zope.authenticated'：登录用户
- 'users.xxx'：公司内的登录用户
- 'clients.xxx'：外部人员

组分为如下几种：

- groups.groups.xxx: 组
- groups.tree.xxx: 组织结构节点, 比如部门, 注意是单层的, 这是出于授权统一的考虑.
- groups.job.xxx : 岗位
- groups.role.AccountOwner : 账户管理员，这个命名固定
- groups.license.app_name-instance_name-service-name: 分配的许可组

内部服务ID
--------------------
- docs : 基础平台
- projects ： 项目
- sites :部门
- sms ：短信

部门与岗位
--------------

部门与岗位有两个比较重要的属性，部门的title，部门的Id.Id可以通过人员选择框获得，而title则需要通过以下这个接口获得，事例代码如下：::

  group_id = context['department'][0]  #人员选择框
  info = org_info.getPrincipalInfo(group_id) 
  """ 得到人员和组基本信息    
     人员: id，title，mobile，email   
     组:  id,title 
  """
  group_title = info['title']
  


团队管理
=================


所谓团队,是非系统管理员管理的临时组，包括项目组、工作组、部门等. 

- 团队只能分2级

  - 总团队，比如一个项目的所有组员 groups.team.[intid]
  - 团队下面的一个子组，比如设计组：groups.team.[intid]-[team_name]
  - 一个人如果属于一个团队，将同时属于上述2个组

- 一旦项目、部门结束，团队解散：

  - 登录用户的reqeust.principal.groups中, 不再包含这些解散组队的信息
  - 针对这些组的授权也同时失效

ITeamManager(context):
---------------------------------

- listTeams(): 返回 [{id, title, description, members}]
- getTeam(id): 得到一个团队的信息 {id, title, description, members}
- getMemberTeams(member): 到人员所在的组id
- setTeamTitle(id, title, description): 设置组员标题和描述
- appendTeamMember(id, member): 添加一个组员
- removeTeamMember(id, member): 删除一个组员
- setTeamMembers(id, members): 设置组员
- addTeam(id, title, description, members): 添加一个团队
- removeTeam(id):删除一个团队
