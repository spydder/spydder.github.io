---
title: "Guia: Sintaxis de plantillas"
date: 2023-12-25
categories: [Templates]
tags: [teamplates, guide]
---

1. Jinja, Django, Flask, Twig:
    - Delimitador de apertura: **{% raw %}{%{% endraw %}**
    - Delimitador de cierre: **{% raw %}%}{% endraw %}**
    - Ejemplo de estructura básica:
        
        ```bash
        {% raw %}
        {% if condition %}
            <p>Esta es una condición verdadera.</p>
        {% else %}
            <p>Esta es una condición falsa.</p>
        {% endif %}
        {% endraw %}
        ```
        
2. Groovy:
    - Delimitador de apertura: **{% raw %}<%{% endraw %}**
    - Delimitador de cierre: **{% raw %}%>{% endraw %}**
    - Ejemplo de estructura básica:
        
        ```bash
        {% raw %}
        <% if (condition) { %>
            <p>Esta es una condición verdadera.</p>
        <% } else { %>
            <p>Esta es una condición falsa.</p>
        <% } %>
        {% endraw %}
        ```
        
3. Smarty:
    - Delimitador de apertura: **{% raw %}{%{% endraw %}**
    - Delimitador de cierre: **{% raw %}%}{% endraw %}**
    - Ejemplo de estructura básica:
        
        ```bash
        {% raw %}
        {if $condition}
            <p>Esta es una condición verdadera.</p>
        {else}
            <p>Esta es una condición falsa.</p>
        {/if}
        {% endraw %}
        ```
        
4. Blade (Laravel):
    - Delimitador de apertura: **@**
    - Delimitador de cierre: **@**
    - Ejemplo de estructura básica:
        
        ```bash
        {% raw %}
        @if(condition)
            <p>Esta es una condición verdadera.</p>
        @else
            <p>Esta es una condición falsa.</p>
        @endif
        {% endraw %}
        ```
        
5. Handlebars:
    - Delimitador de apertura: **{% raw %}{{{% endraw %}**
    - Delimitador de cierre: **{% raw %}}}{% endraw %}**
    - Ejemplo de estructura básica:
        
        ```bash
        {% raw %}
        {{#if condition}}
            <p>Esta es una condición verdadera.</p>
        {{else}}
            <p>Esta es una condición falsa.</p>
        {{/if}}
        {% endraw %}
        ```
        
6. Mustache:
    - Delimitador de apertura: **{% raw %}{{{% endraw %}**
    - Delimitador de cierre: **{% raw %}}}{% endraw %}**
    - Ejemplo de estructura básica:
        
        ```bash
        {% raw %}
        {{#condition}}
            <p>Esta es una condición verdadera.</p>
        {{else}}
            <p>Esta es una condición falsa.</p>
        {{/condition}}
        {% endraw %}
        ```
        
7. EJS (Embedded JavaScript):
    - Delimitador de apertura: **{% raw %}<%{% endraw %}**
    - Delimitador de cierre: **{% raw %}%>{% endraw %}**
    - Ejemplo de estructura básica:
        
        ```bash
        {% raw %}
        <% if (condition) { %>
            <p>Esta es una condición verdadera.</p>
        <% } else { %>
            <p>Esta es una condición falsa.</p>
        <% } %>
        {% endraw %}
        ```
        
8. Velocity:
    - Delimitador de apertura: **#**
    - Delimitador de cierre: ninguno (basado en indentación)
    - Ejemplo de estructura básica:
        
        ```bash
        {% raw %}
        #if($condition)
            <p>Esta es una condición verdadera.</p>
        #else
            <p>Esta es una condición falsa.</p>
        #end
        {% endraw %}
        ```
