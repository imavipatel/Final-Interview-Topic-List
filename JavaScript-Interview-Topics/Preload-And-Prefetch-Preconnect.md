# Preload, Prefetch, Preconnect

- Today I'm showing the simple techniques to make your site loads fast.

- First, we need to know what is Preload, Preconnect, and Prefetch.

# Preload

- When we use **preload in link tag** it makes **early fetch request** to get the resource. Mostly used to **fetch high priority resource** that is used in the current route

# Preconnect:

- It resolves the DNS and TCP handshaking.

# DNS-Preconnect:

- It only resolves the DNS.

# Prefetch:

- It helps to fetch the resources and place it in the cache.
- whenever the resources might need it will take it from the cache instead of making another request.

**preload is used for the high priority resources and prefetch is used for the low priority resources.**
