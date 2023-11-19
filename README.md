# Домашнее задание к занятию «Продвинутые методы работы с Terraform» - Сергей Ситкарёв

## Задание 1

Возьмите из демонстрации к лекции готовый код для создания ВМ с помощью remote-модуля.
Создайте одну ВМ, используя этот модуль. В файле cloud-init.yml необходимо использовать переменную для ssh-ключа вместо хардкода.
Добавьте в файл cloud-init.yml установку nginx.

![Задание1](https://github.com/SSitkarev/terraform-04/blob/main/img/1.jpg)

Передайте ssh-ключ в функцию template_file в блоке vars ={} . Воспользуйтесь примером. Обратите внимание, что ssh-authorized-keys принимает в себя список, а не строку.

![Задание1](https://github.com/SSitkarev/terraform-04/blob/main/img/2-1.jpg)

Предоставьте скриншот подключения к консоли и вывод команды sudo nginx -t.

![Задание1](https://github.com/SSitkarev/terraform-04/blob/main/img/3.jpg)

## Задание 2

Напишите локальный модуль vpc, который будет создавать 2 ресурса: одну сеть и одну подсеть в зоне, объявленной при вызове модуля, например: ru-central1-a.

![Задание2](https://github.com/SSitkarev/terraform-04/blob/main/img/4.jpg)

Вы должны передать в модуль переменные с названием сети, zone и v4_cidr_blocks.
Модуль должен возвращать в root module с помощью output информацию о yandex_vpc_subnet. Пришлите скриншот информации из terraform console о своем модуле. Пример: > module.vpc_dev
Замените ресурсы yandex_vpc_network и yandex_vpc_subnet созданным модулем. Не забудьте передать необходимые параметры сети из модуля vpc в модуль с виртуальной машиной.

![Задание2](https://github.com/SSitkarev/terraform-04/blob/main/img/7.jpg)

Откройте terraform console и предоставьте скриншот содержимого модуля. Пример: > module.vpc_dev.

![Задание2](https://github.com/SSitkarev/terraform-04/blob/main/img/6.jpg)

Сгенерируйте документацию к модулю с помощью terraform-docs.

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >=0.13 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_template"></a> [template](#provider\_template) | 2.2.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_test-vm"></a> [test-vm](#module\_test-vm) | git::https://github.com/udjin10/yandex_compute_instance.git | main |
| <a name="module_vpc_dev"></a> [vpc\_dev](#module\_vpc\_dev) | ./vpc | n/a |

## Resources

| Name | Type |
|------|------|
| [template_file.cloudinit](https://registry.terraform.io/providers/hashicorp/template/latest/docs/data-sources/file) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_cloud_id"></a> [cloud\_id](#input\_cloud\_id) | https://cloud.yandex.ru/docs/resource-manager/operations/cloud/get-id | `string` | n/a | yes |
| <a name="input_default_cidr"></a> [default\_cidr](#input\_default\_cidr) | https://cloud.yandex.ru/docs/vpc/operations/subnet-create | `list(string)` | <pre>[<br>  "10.0.1.0/24"<br>]</pre> | no |
| <a name="input_default_zone"></a> [default\_zone](#input\_default\_zone) | https://cloud.yandex.ru/docs/overview/concepts/geo-scope | `string` | `"ru-central1-a"` | no |
| <a name="input_folder_id"></a> [folder\_id](#input\_folder\_id) | https://cloud.yandex.ru/docs/resource-manager/operations/folder/get-id | `string` | n/a | yes |
| <a name="input_ssh-keys"></a> [ssh-keys](#input\_ssh-keys) | ssh key path | `list(string)` | <pre>[<br>  "C:/Users/Sergey/.ssh/id_rsa.pub"<br>]</pre> | no |
| <a name="input_token"></a> [token](#input\_token) | OAuth-token; https://cloud.yandex.ru/docs/iam/concepts/authorization/oauth-token | `string` | n/a | yes |
| <a name="input_vpc_name"></a> [vpc\_name](#input\_vpc\_name) | VPC network&subnet name | `string` | `"develop"` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_dev_network_id"></a> [dev\_network\_id](#output\_dev\_network\_id) | n/a |
| <a name="output_dev_subnet_id"></a> [dev\_subnet\_id](#output\_dev\_subnet\_id) | n/a |


## Задание 3

Выведите список ресурсов в стейте.

Полностью удалите из стейта модуль vpc.

Полностью удалите из стейта модуль vm.

Импортируйте всё обратно. Проверьте terraform plan. Изменений быть не должно. Приложите список выполненных команд и скриншоты процессы.

## Задание 6*

Разверните у себя локально vault, используя docker-compose.yml в проекте.
Для входа в web-интерфейс и авторизации terraform в vault используйте токен "education".
Создайте новый секрет по пути http://127.0.0.1:8200/ui/vault/secrets/secret/create Path: example
secret data key: test secret data value: congrats!
Считайте этот секрет с помощью terraform и выведите его в output по примеру:
provider "vault" {
 address = "http://<IP_ADDRESS>:<PORT_NUMBER>"
 skip_tls_verify = true
 token = "education"
}
data "vault_generic_secret" "vault_example"{
 path = "secret/example"
}

output "vault_example" {
 value = "${nonsensitive(data.vault_generic_secret.vault_example.data)}"
} 

Можно обратиться не к словарю, а конкретному ключу:
terraform console: >nonsensitive(data.vault_generic_secret.vault_example.data.<имя ключа в секрете>)
Попробуйте самостоятельно разобраться в документации и записать новый секрет в vault с помощью terraform.