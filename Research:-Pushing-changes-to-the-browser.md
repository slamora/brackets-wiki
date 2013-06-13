Inspector DOM API docs https://developers.google.com/chrome-developer-tools/docs/protocol/tot/dom

Notes
----

* ``DOM.getDocument()`` only returns a node tree to the depth of ``body``
* When using ``DOM.requestChildNodes`` result data is signaled via the ``DOM.setChildNodes`` event
* Same node is never sent twice, i.e. sequential calls to ``DOM.getDocument()`` will return the same tree but with different ``nodeId``