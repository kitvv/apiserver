#Loopback sample appliction

##how to create model
please go through the 
http://loopback.io

##how to define relationship
please check /models/*.json 
####in this sample 3 main relationship was defined
+ user own one or more vendors
+ vendor own one or more products
+ user own one or more transactions

##how to define roles
please check /server/boot/role-resolver.js
####in this sample 2 roles are defined
+ bank:predefined email address will be treated as bank user 
+ vendor:user who owns 1 or more vendors will be treated as vendor 

##how to define authentication
####in this sample these access rules are defined
1.  /models/system_user.json 
+ everyone can login
+ only bank role can issueCoin ,it can be issued to any id
+ only bank role can destroyCoin , it can be only destroyed from bank id
+ user can only edit his own information
+ only bank role can list all users
+ registered user can create vendor and it will belongs to him
+ user can only list all his vendors
+ vendor information can be only edited by owner
+ registered user invoke transaction
+ registered user can list all transaction related to him

2.  /models/vendor.json 
+ vendor owner can create product belongs to one of his vendor
+ vendor owner can list all products via vendor id

3.  for product and transaction model , deny all access

##how to add system property
####in this sample createtimestamp and modify timestamp were defined to vendor model
1.  add your js to /common/mixins. in this sample timeStamp.js was added
2.  edit your target model.json. in this sample vendo.json
<code>
"mixins": {
    "TimeStamp": true
  }
 </code> was added. please be noted the first t need to be T. not know why
 

##how to add user api
<p>Here is an example for API POST /issueCoin </p>
<pre><code>
System_user.issueCoin = function(id, amount, cb) {
    System_user.findById(id, function(err, targetuser) {
      if (err) return cb(err);
  };
  System_user.remoteMethod('issueCoin', {
    accepts: [
      {arg: 'id', type: 'number'},
      {arg: 'amount', type: 'number'},
    ],
    description :"Issue coin by bank user to target user id" ,
    returns: {arg: 'after_process', type: 'array'},
    http: {path:'/issueCoin', verb: 'post'}
  });
</code></pre>
check /common/models/system_user.js for more api

##how to add remote hook
<p>Here is an example for excuted before user model created</p>
<pre><code>
System_user.beforeRemote('create', function(context, user, next) {
    context.args.data = dataformat.userformat(context.args.data);
    next();
  });
</code></pre>

