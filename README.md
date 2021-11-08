# devops-netology
# Будет проигнорировано содержимое поддиректорий .terraform
**/.terraform/*

# Будут проигнорированы все файлы, содержащие в названии .tfstate и .tfstate.
*.tfstate
*.tfstate.*

# Будут проигнорированы рекурсивно все файлы crash.log
crash.log

# Будут проигнорированы рекурсивно все файлы с расширением .tfvars
*.tfvars

# Будут проигнорированы рекурсивно файлы с именами override.tf и override.tf.jsont
override.tf
override.tf.json
# Будут проигнорированы рекурсивно файлы, содержащие в имени _override.tf и _override.tf.json
*_override.tf
*_override.tf.json

# Будут проигнорированы рекурсивно файлы .terraformrc и terraform.rc 
.terraformrc
terraform.rc
File is modified!

Another line in branch main. It is absent in branch fix!
One more new line in branch main