---
title: "Usando layout (tema) diferente nas views do devise"
date: 2016-05-06 10:00:00
description: Definindo layout específico para o devise
---

Recentemente precisei usar um tema diferente na tela de login de uma aplicação usando o devise.  Por padrão todas as views geradas pelo Rails possuem como layout o application.html.erb
{% highlight ruby %}
app/views/layouts/application.html.erb
{% endhighlight %}

### Criando um layout específico

A primeira e mais fácil é simplesmente criar um layout com o nome de devise na pasta das views que siga a mesma lógica definida no application.html.erb
{% highlight ruby %}
app/views/layouts/devise.html.erb
{% endhighlight %}

Com isso, todas as views geradas pelo devise (registrations, sessions, confirmations) usarão especificamente esse layout, permitindo o uso de um tema css, ou de scripts javascript únicos para as views de Login, Logout e etc.

### Método no controller

Outra forma  é definir um método no application controller para determinar quando um controller do devise está ativo e responder de acordo com isto:

{% highlight ruby %}
class ApplicationController < ActionController::Base
  layout :layout_by_resource

  protected

  def layout_by_resource
    if devise_controller?
      "nome_do_layout_para_o_devise"
    else
      "application"
    end
  end
end
{% endhighlight %}

### Definir nas configurações

Outra opção é usar um callback dentro do bloco de configurações no config/application.rb:

{% highlight ruby %}
config.to_prepare do
  Devise::SessionsController.layout "nomedolayout"
end
{% endhighlight %}

É sempre bom ressaltar o excelente guia do [rails](http://guides.rubyonrails.org/layouts_and_rendering.html), e a também excelente documentação do [devise](https://github.com/plataformatec/devise/wiki)
