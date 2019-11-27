---
title: How to Return JSON-Encoded Response for non-dict object
date: 2019-11-27 23:17:00 +05:30
categories:
- blog
tags:
- django
- beginners
- python
- tutorial
category: blog
---

An HttpResponse subclass that helps to create a JSON-encoded response. Its default Content-Type header is set to application/JSON.

The first parameter, data, should be a dict instance. If non-dict object is passed as the first argument, a TypeError will be raised.

#### Example dict instance:
``` python

from django.http import JsonResponse

def user(req):
    response = {
        'id': 1,
        'name': 'Tom',
        'status': Active
    }
    return JsonResponse(response)
```

#### Example non-dict object:
```
return JsonResponse([1, 2, 3, 4])
```

TypeError: In order to allow non-dict objects to be serialized set the safe parameter to False

By default, the JsonResponse’s first parameter, data, should be a dict instance. To pass any other JSON-serializable object you must set the safe parameter to False.

```
return JsonResponse([1, 2, 3, 4], safe=False)
```

The safe boolean parameter defaults to True. If it’s set to False, any object can be passed for serialization (otherwise only dict instances are allowed).

