helm package myapp/
curl --data-binary "@myapp-0.1.1.tgz" http://localhost:8084/api/charts

Apoi am facut o mica modificare pe template si in Charts.yaml la version si din nou upload. Astel avem 2 versiuni de myapp

helm search repo -l chartmuseumrepo -> vedem ce chart-uri avem acolo

!!!atentie,e posibil sa ai probleme cu case sensitive issue asa ca trebuie sa faci totul cu litere mici
Chiar daca atunci cand dai: helm search repo chartmuseum o sa vezi cu litere mici

Pentru verificare:

helm install test --dry-run --debug chartmuseumrepo/HELM-CONFIGMAP --version 0.2.0
helm install test --dry-run --debug chartmuseumrepo/HELM-CONFIGMAP --version 0.1.0

helm install test --debug chartmuseumrepo/HELM-CONFIGMAP --version 0.1.0
helm upgrade test --debug chartmuseumrepo/HELM-CONFIGMAP --version 0.2.0

helm history test -> vezi cand au fost deployate chart-urile
helm get values --revision=1 test
helm get manifest test  -> vezi cum arata fisierele yaml ce ruleaza acum
helm get manifest test --revision 1 -> cum aratau fisierele yaml ce au fost aplicate la versiune precedenta

#####################
ROLlBACK

helm install test chartmuseumrepo/myapp --version 0.1.2
helm upgrade test --debug chartmuseumrepo/myapp --version 0.1.3
helm history test

rollback: helm rollback test 2 -> rollback to revision 2


####################
Insert custom values
helm create test
helm install test-2 --dry-run --debug test/ --set service.type=NodePort

####################

You can also add hooks that will be triggered when you want based on annotations ex presinstall, postinstall etc
You can also use weight triggers based on particular patterns