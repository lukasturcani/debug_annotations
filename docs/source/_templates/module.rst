{{ fullname | escape | underline}}

.. automodule:: {{ fullname }}

   {% block classes %}

   .. autosummary::
      :toctree:
      :template: class.rst
      :nosignatures:
   {% for item in classes %}
      {{ item }}
   {%- endfor %}
   {% endblock %}
