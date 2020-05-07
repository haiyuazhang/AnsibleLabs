This lab aims to demenostrate how to use azure_rm_managedopenshiftcluster to create/delete a openshift managed cluster.

step1: create a service principal
you can either create a service principal via azure cli or directly in the portal.After that,
1) set the client_id in the vars.yml to the client id of the newly created SP.
2) set the client_secret int the vars.yml to the client secret of the newly created SP.
3) set the USER_PROVIDED_SP_OBJID to the object id of the newly created SP.

step2: run prepare.sh
first, set your subscription id in use to the SUBID variable in prepare.sh. that is the only mandatory change
needed.you may change other various setting at the begenning part of the script, but Don't touch BUILDIN_SP_OBJID,
leave what it is.

Then simply run the script. the script will emit serval key-value settings at the end of the execution,
update corronsponding settings in vars.yml according to what you got from the execution output.

step3: run create-cluster.yml
Run  "ansible-playbook -v create-cluster.yml" to create an openshift cluster. 

It will takes around 40 minutes to finish if no errors occur during the execution. you can check the final cluster provisioning state in azure portal.If the creation failed, the resource group activily log usually will give you good hints of what has went wrong.

step4: run delete-cluster.yml
use command anisble-playbook -v delete-cluster.yml to delete an existing openshift cluster. 

Updating the cluster is not yet supported by the module. If you check new openshift api by yourself, you'll find
almost all the fields defined in the api model are immutable, So evnth though it is possible that 
fileds like tags is updatable, but really doesn't bring any practical value, so I just don't bother to test all the
fields to see which are updatable. And, if you run az aro update --help, you'll find no updatable options in the help output content, which let me further belive my general judegment that the current api design is not supposed to update a cluster is correct.

