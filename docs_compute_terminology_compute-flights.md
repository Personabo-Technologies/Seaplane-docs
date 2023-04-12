<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Compute
-   Managed Global Compute Terminology
-   What are flights?

<div>

On this page

</div>

<div>

<div>

# What are flights?

</div>

When using *Seaplane Managed Global Compute* (*MGC*) there are two key
terms you need to know: *flights* and *formations*. This document dives
into the technical details of *flights*. You can learn more about
*formations* here.

## Flights vs Formations​

Before we dive into the specific details of flights, it\'s important to
understand the difference between a *flight* and a *formation*.

A *flight* consists of a single container image. Under the hood,
Seaplane can spin up many container instances and spread them around the
globe or within the regions you specify (more on that later).

A *formation* consists of one or more flights. *Formations* can be
thought of as your high-level application or service. Formations include
the load balancer and entry-point (resource URL) to your service. This
is the entry point for all public and private traffic.

## Flights​

Like we said before, flights are only the container image definition.
It\'s important to note that *flights* cannot be deployed by themselves,
they must be part of a *formation*. Only formations can be deployed,
because only formations define routing and load balancing to container
instances of the referenced flight(s).

Think of it like air traffic control (because we love plane metaphors
around here): a plane can\'t take off willy nilly from your nearest
regional airport. It needs to have a planned route, approved by air
traffic control, to ensure it winds up in the right place without
interrupting all the other planes in the air.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48c3R5bGU+I21lcm1haWQtc3ZnLTY3NDU3ODR7Zm9udC1mYW1pbHk6InRyZWJ1Y2hldCBtcyIsdmVyZGFuYSxhcmlhbCxzYW5zLXNlcmlmO2ZvbnQtc2l6ZToxNnB4O2ZpbGw6IzMzMzt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmVycm9yLWljb257ZmlsbDojNTUyMjIyO30jbWVybWFpZC1zdmctNjc0NTc4NCAuZXJyb3ItdGV4dHtmaWxsOiM1NTIyMjI7c3Ryb2tlOiM1NTIyMjI7fSNtZXJtYWlkLXN2Zy02NzQ1Nzg0IC5lZGdlLXRoaWNrbmVzcy1ub3JtYWx7c3Ryb2tlLXdpZHRoOjJweDt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmVkZ2UtdGhpY2tuZXNzLXRoaWNre3N0cm9rZS13aWR0aDozLjVweDt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmVkZ2UtcGF0dGVybi1zb2xpZHtzdHJva2UtZGFzaGFycmF5OjA7fSNtZXJtYWlkLXN2Zy02NzQ1Nzg0IC5lZGdlLXBhdHRlcm4tZGFzaGVke3N0cm9rZS1kYXNoYXJyYXk6Mzt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmVkZ2UtcGF0dGVybi1kb3R0ZWR7c3Ryb2tlLWRhc2hhcnJheToyO30jbWVybWFpZC1zdmctNjc0NTc4NCAubWFya2Vye2ZpbGw6IzMzMzMzMztzdHJva2U6IzMzMzMzMzt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLm1hcmtlci5jcm9zc3tzdHJva2U6IzMzMzMzMzt9I21lcm1haWQtc3ZnLTY3NDU3ODQgc3Zne2ZvbnQtZmFtaWx5OiJ0cmVidWNoZXQgbXMiLHZlcmRhbmEsYXJpYWwsc2Fucy1zZXJpZjtmb250LXNpemU6MTZweDt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmxhYmVse2ZvbnQtZmFtaWx5OiJ0cmVidWNoZXQgbXMiLHZlcmRhbmEsYXJpYWwsc2Fucy1zZXJpZjtjb2xvcjojMzMzO30jbWVybWFpZC1zdmctNjc0NTc4NCAuY2x1c3Rlci1sYWJlbCB0ZXh0e2ZpbGw6IzMzMzt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmNsdXN0ZXItbGFiZWwgc3Bhbntjb2xvcjojMzMzO30jbWVybWFpZC1zdmctNjc0NTc4NCAubGFiZWwgdGV4dCwjbWVybWFpZC1zdmctNjc0NTc4NCBzcGFue2ZpbGw6IzMzMztjb2xvcjojMzMzO30jbWVybWFpZC1zdmctNjc0NTc4NCAubm9kZSByZWN0LCNtZXJtYWlkLXN2Zy02NzQ1Nzg0IC5ub2RlIGNpcmNsZSwjbWVybWFpZC1zdmctNjc0NTc4NCAubm9kZSBlbGxpcHNlLCNtZXJtYWlkLXN2Zy02NzQ1Nzg0IC5ub2RlIHBvbHlnb24sI21lcm1haWQtc3ZnLTY3NDU3ODQgLm5vZGUgcGF0aHtmaWxsOiNFQ0VDRkY7c3Ryb2tlOiM5MzcwREI7c3Ryb2tlLXdpZHRoOjFweDt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLm5vZGUgLmxhYmVse3RleHQtYWxpZ246Y2VudGVyO30jbWVybWFpZC1zdmctNjc0NTc4NCAubm9kZS5jbGlja2FibGV7Y3Vyc29yOnBvaW50ZXI7fSNtZXJtYWlkLXN2Zy02NzQ1Nzg0IC5hcnJvd2hlYWRQYXRoe2ZpbGw6IzMzMzMzMzt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmVkZ2VQYXRoIC5wYXRoe3N0cm9rZTojMzMzMzMzO3N0cm9rZS13aWR0aDoyLjBweDt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmZsb3djaGFydC1saW5re3N0cm9rZTojMzMzMzMzO2ZpbGw6bm9uZTt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmVkZ2VMYWJlbHtiYWNrZ3JvdW5kLWNvbG9yOiNlOGU4ZTg7dGV4dC1hbGlnbjpjZW50ZXI7fSNtZXJtYWlkLXN2Zy02NzQ1Nzg0IC5lZGdlTGFiZWwgcmVjdHtvcGFjaXR5OjAuNTtiYWNrZ3JvdW5kLWNvbG9yOiNlOGU4ZTg7ZmlsbDojZThlOGU4O30jbWVybWFpZC1zdmctNjc0NTc4NCAuY2x1c3RlciByZWN0e2ZpbGw6I2ZmZmZkZTtzdHJva2U6I2FhYWEzMztzdHJva2Utd2lkdGg6MXB4O30jbWVybWFpZC1zdmctNjc0NTc4NCAuY2x1c3RlciB0ZXh0e2ZpbGw6IzMzMzt9I21lcm1haWQtc3ZnLTY3NDU3ODQgLmNsdXN0ZXIgc3Bhbntjb2xvcjojMzMzO30jbWVybWFpZC1zdmctNjc0NTc4NCBkaXYubWVybWFpZFRvb2x0aXB7cG9zaXRpb246YWJzb2x1dGU7dGV4dC1hbGlnbjpjZW50ZXI7bWF4LXdpZHRoOjIwMHB4O3BhZGRpbmc6MnB4O2ZvbnQtZmFtaWx5OiJ0cmVidWNoZXQgbXMiLHZlcmRhbmEsYXJpYWwsc2Fucy1zZXJpZjtmb250LXNpemU6MTJweDtiYWNrZ3JvdW5kOmhzbCg4MCwgMTAwJSwgOTYuMjc0NTA5ODAzOSUpO2JvcmRlcjoxcHggc29saWQgI2FhYWEzMztib3JkZXItcmFkaXVzOjJweDtwb2ludGVyLWV2ZW50czpub25lO3otaW5kZXg6MTAwO30jbWVybWFpZC1zdmctNjc0NTc4NCAuZmxvd2NoYXJ0VGl0bGVUZXh0e3RleHQtYW5jaG9yOm1pZGRsZTtmb250LXNpemU6MThweDtmaWxsOiMzMzM7fSNtZXJtYWlkLXN2Zy02NzQ1Nzg0IDpyb290ey0tbWVybWFpZC1mb250LWZhbWlseToidHJlYnVjaGV0IG1zIix2ZXJkYW5hLGFyaWFsLHNhbnMtc2VyaWY7fTwvc3R5bGU+PGc+PG1hcmtlcj48cGF0aD48L3BhdGg+PC9tYXJrZXI+PG1hcmtlcj48cGF0aD48L3BhdGg+PC9tYXJrZXI+PG1hcmtlcj48Y2lyY2xlPjwvY2lyY2xlPjwvbWFya2VyPjxtYXJrZXI+PGNpcmNsZT48L2NpcmNsZT48L21hcmtlcj48bWFya2VyPjxwYXRoPjwvcGF0aD48L21hcmtlcj48bWFya2VyPjxwYXRoPjwvcGF0aD48L21hcmtlcj48Zz48Zz48Zz48cmVjdD48L3JlY3Q+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj5Gb3JtYXRpb248L3NwYW4+PC9kaXY+PC9mb3JlaWdub2JqZWN0PjwvZz48L2c+PC9nPjxnPjxwYXRoPjwvcGF0aD48cGF0aD48L3BhdGg+PHBhdGg+PC9wYXRoPjxwYXRoPjwvcGF0aD48L2c+PGc+PGc+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj48L3NwYW4+PC9kaXY+PC9mb3JlaWdub2JqZWN0PjwvZz48L2c+PGc+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj48L3NwYW4+PC9kaXY+PC9mb3JlaWdub2JqZWN0PjwvZz48L2c+PGc+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj48L3NwYW4+PC9kaXY+PC9mb3JlaWdub2JqZWN0PjwvZz48L2c+PGc+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj48L3NwYW4+PC9kaXY+PC9mb3JlaWdub2JqZWN0PjwvZz48L2c+PC9nPjxnPjxnPjxyZWN0PjwvcmVjdD48Zz48Zm9yZWlnbm9iamVjdD48ZGl2PjxzcGFuPjxpPjwvaT4gRmxpZ2h0Qjwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48Zz48cmVjdD48L3JlY3Q+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj48aT48L2k+IEZsaWdodEE8L3NwYW4+PC9kaXY+PC9mb3JlaWdub2JqZWN0PjwvZz48L2c+PGc+PHJlY3Q+PC9yZWN0PjxnPjxmb3JlaWdub2JqZWN0PjxkaXY+PHNwYW4+PGk+PC9pPiBGbGlnaHRDPC9zcGFuPjwvZGl2PjwvZm9yZWlnbm9iamVjdD48L2c+PC9nPjxnPjxyZWN0PjwvcmVjdD48Zz48Zm9yZWlnbm9iamVjdD48ZGl2PjxzcGFuPjxpPjwvaT4gRmxpZ2h0RDwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48Zz48cG9seWdvbj48L3BvbHlnb24+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj48aT48L2k+IFVzZXJzPC9zcGFuPjwvZGl2PjwvZm9yZWlnbm9iamVjdD48L2c+PC9nPjwvZz48L2c+PC9nPjwvc3ZnPg==)

</div>

</div>

If all that sounds a bit too theoretical, let\'s look at a real-world
example. Imagine you\'re setting up an ecommerce website. Like many
modern web applications, your website will be made up of a few different
components; a front-facing webserver, a backend database, and maybe a
backend metrics platform. If you were to make a diagram showing how they
all connect, it would look something like this:

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48c3R5bGU+I21lcm1haWQtc3ZnLTI2MzA3ODh7Zm9udC1mYW1pbHk6InRyZWJ1Y2hldCBtcyIsdmVyZGFuYSxhcmlhbCxzYW5zLXNlcmlmO2ZvbnQtc2l6ZToxNnB4O2ZpbGw6IzMzMzt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmVycm9yLWljb257ZmlsbDojNTUyMjIyO30jbWVybWFpZC1zdmctMjYzMDc4OCAuZXJyb3ItdGV4dHtmaWxsOiM1NTIyMjI7c3Ryb2tlOiM1NTIyMjI7fSNtZXJtYWlkLXN2Zy0yNjMwNzg4IC5lZGdlLXRoaWNrbmVzcy1ub3JtYWx7c3Ryb2tlLXdpZHRoOjJweDt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmVkZ2UtdGhpY2tuZXNzLXRoaWNre3N0cm9rZS13aWR0aDozLjVweDt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmVkZ2UtcGF0dGVybi1zb2xpZHtzdHJva2UtZGFzaGFycmF5OjA7fSNtZXJtYWlkLXN2Zy0yNjMwNzg4IC5lZGdlLXBhdHRlcm4tZGFzaGVke3N0cm9rZS1kYXNoYXJyYXk6Mzt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmVkZ2UtcGF0dGVybi1kb3R0ZWR7c3Ryb2tlLWRhc2hhcnJheToyO30jbWVybWFpZC1zdmctMjYzMDc4OCAubWFya2Vye2ZpbGw6IzMzMzMzMztzdHJva2U6IzMzMzMzMzt9I21lcm1haWQtc3ZnLTI2MzA3ODggLm1hcmtlci5jcm9zc3tzdHJva2U6IzMzMzMzMzt9I21lcm1haWQtc3ZnLTI2MzA3ODggc3Zne2ZvbnQtZmFtaWx5OiJ0cmVidWNoZXQgbXMiLHZlcmRhbmEsYXJpYWwsc2Fucy1zZXJpZjtmb250LXNpemU6MTZweDt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmxhYmVse2ZvbnQtZmFtaWx5OiJ0cmVidWNoZXQgbXMiLHZlcmRhbmEsYXJpYWwsc2Fucy1zZXJpZjtjb2xvcjojMzMzO30jbWVybWFpZC1zdmctMjYzMDc4OCAuY2x1c3Rlci1sYWJlbCB0ZXh0e2ZpbGw6IzMzMzt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmNsdXN0ZXItbGFiZWwgc3Bhbntjb2xvcjojMzMzO30jbWVybWFpZC1zdmctMjYzMDc4OCAubGFiZWwgdGV4dCwjbWVybWFpZC1zdmctMjYzMDc4OCBzcGFue2ZpbGw6IzMzMztjb2xvcjojMzMzO30jbWVybWFpZC1zdmctMjYzMDc4OCAubm9kZSByZWN0LCNtZXJtYWlkLXN2Zy0yNjMwNzg4IC5ub2RlIGNpcmNsZSwjbWVybWFpZC1zdmctMjYzMDc4OCAubm9kZSBlbGxpcHNlLCNtZXJtYWlkLXN2Zy0yNjMwNzg4IC5ub2RlIHBvbHlnb24sI21lcm1haWQtc3ZnLTI2MzA3ODggLm5vZGUgcGF0aHtmaWxsOiNFQ0VDRkY7c3Ryb2tlOiM5MzcwREI7c3Ryb2tlLXdpZHRoOjFweDt9I21lcm1haWQtc3ZnLTI2MzA3ODggLm5vZGUgLmxhYmVse3RleHQtYWxpZ246Y2VudGVyO30jbWVybWFpZC1zdmctMjYzMDc4OCAubm9kZS5jbGlja2FibGV7Y3Vyc29yOnBvaW50ZXI7fSNtZXJtYWlkLXN2Zy0yNjMwNzg4IC5hcnJvd2hlYWRQYXRoe2ZpbGw6IzMzMzMzMzt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmVkZ2VQYXRoIC5wYXRoe3N0cm9rZTojMzMzMzMzO3N0cm9rZS13aWR0aDoyLjBweDt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmZsb3djaGFydC1saW5re3N0cm9rZTojMzMzMzMzO2ZpbGw6bm9uZTt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmVkZ2VMYWJlbHtiYWNrZ3JvdW5kLWNvbG9yOiNlOGU4ZTg7dGV4dC1hbGlnbjpjZW50ZXI7fSNtZXJtYWlkLXN2Zy0yNjMwNzg4IC5lZGdlTGFiZWwgcmVjdHtvcGFjaXR5OjAuNTtiYWNrZ3JvdW5kLWNvbG9yOiNlOGU4ZTg7ZmlsbDojZThlOGU4O30jbWVybWFpZC1zdmctMjYzMDc4OCAuY2x1c3RlciByZWN0e2ZpbGw6I2ZmZmZkZTtzdHJva2U6I2FhYWEzMztzdHJva2Utd2lkdGg6MXB4O30jbWVybWFpZC1zdmctMjYzMDc4OCAuY2x1c3RlciB0ZXh0e2ZpbGw6IzMzMzt9I21lcm1haWQtc3ZnLTI2MzA3ODggLmNsdXN0ZXIgc3Bhbntjb2xvcjojMzMzO30jbWVybWFpZC1zdmctMjYzMDc4OCBkaXYubWVybWFpZFRvb2x0aXB7cG9zaXRpb246YWJzb2x1dGU7dGV4dC1hbGlnbjpjZW50ZXI7bWF4LXdpZHRoOjIwMHB4O3BhZGRpbmc6MnB4O2ZvbnQtZmFtaWx5OiJ0cmVidWNoZXQgbXMiLHZlcmRhbmEsYXJpYWwsc2Fucy1zZXJpZjtmb250LXNpemU6MTJweDtiYWNrZ3JvdW5kOmhzbCg4MCwgMTAwJSwgOTYuMjc0NTA5ODAzOSUpO2JvcmRlcjoxcHggc29saWQgI2FhYWEzMztib3JkZXItcmFkaXVzOjJweDtwb2ludGVyLWV2ZW50czpub25lO3otaW5kZXg6MTAwO30jbWVybWFpZC1zdmctMjYzMDc4OCAuZmxvd2NoYXJ0VGl0bGVUZXh0e3RleHQtYW5jaG9yOm1pZGRsZTtmb250LXNpemU6MThweDtmaWxsOiMzMzM7fSNtZXJtYWlkLXN2Zy0yNjMwNzg4IDpyb290ey0tbWVybWFpZC1mb250LWZhbWlseToidHJlYnVjaGV0IG1zIix2ZXJkYW5hLGFyaWFsLHNhbnMtc2VyaWY7fTwvc3R5bGU+PGc+PG1hcmtlcj48cGF0aD48L3BhdGg+PC9tYXJrZXI+PG1hcmtlcj48cGF0aD48L3BhdGg+PC9tYXJrZXI+PG1hcmtlcj48Y2lyY2xlPjwvY2lyY2xlPjwvbWFya2VyPjxtYXJrZXI+PGNpcmNsZT48L2NpcmNsZT48L21hcmtlcj48bWFya2VyPjxwYXRoPjwvcGF0aD48L21hcmtlcj48bWFya2VyPjxwYXRoPjwvcGF0aD48L21hcmtlcj48Zz48Zz48L2c+PGc+PHBhdGg+PC9wYXRoPjxwYXRoPjwvcGF0aD48cGF0aD48L3BhdGg+PHBhdGg+PC9wYXRoPjxwYXRoPjwvcGF0aD48L2c+PGc+PGc+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj5WaXNpdHMgV2Vic2l0ZTwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48Zz48Zz48Zm9yZWlnbm9iamVjdD48ZGl2PjxzcGFuPkNoZWNrIEludmVudG9yeTwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48Zz48Zz48Zm9yZWlnbm9iamVjdD48ZGl2PjxzcGFuPjwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48Zz48Zz48Zm9yZWlnbm9iamVjdD48ZGl2PjxzcGFuPlNlbmQgTWV0cmljczwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48Zz48Zz48Zm9yZWlnbm9iamVjdD48ZGl2PjxzcGFuPlNlbmQgTWV0cmljczwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48L2c+PGc+PGc+PHBvbHlnb24+PC9wb2x5Z29uPjxnPjxmb3JlaWdub2JqZWN0PjxkaXY+PHNwYW4+PGk+PC9pPiBVc2Vyczwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48Zz48cmVjdD48L3JlY3Q+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj48aT48L2k+IFdlYiBTZXJ2ZXI8L3NwYW4+PC9kaXY+PC9mb3JlaWdub2JqZWN0PjwvZz48L2c+PGc+PHJlY3Q+PC9yZWN0PjxnPjxmb3JlaWdub2JqZWN0PjxkaXY+PHNwYW4+PGk+PC9pPiBEYXRhYmFzZTwvc3Bhbj48L2Rpdj48L2ZvcmVpZ25vYmplY3Q+PC9nPjwvZz48Zz48cmVjdD48L3JlY3Q+PGc+PGZvcmVpZ25vYmplY3Q+PGRpdj48c3Bhbj48aT48L2k+IE1ldHJpY3M8L3NwYW4+PC9kaXY+PC9mb3JlaWdub2JqZWN0PjwvZz48L2c+PC9nPjwvZz48L2c+PC9zdmc+)

</div>

</div>

In this example, both the web server and the metrics platform are
flights. However, the database is not, as flights are always stateless.
If you\'re interested in a global database to use in conjunction with
MGC, take a look at our beta products and request early access here.

### Scaling Flights​

Depending on your *formation* configuration and user load, you may have
multiple container instances spread all over the world backing a single
*flight*. Put simply, more user load = more container instances backing
your *flight*. In this way, each flight within Seaplane is essentially
its own CDN-like cluster optimized for your end user traffic.

### Flight Configurations​

Most of the configuration, such as provider restrictions, regions
restrictions, and specific routing settings, are handled by the
*formation*. The only configuration specifications you can set at the
flight-level is the architecture. If no architecture is provided,
Seaplane will auto-detect the architecture using the image definition.

</div>

<div>

<div>

**Tags:**

-   flights
-   containers
-   compute
-   terminology
-   compute-terminology

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48Zz48cGF0aD48L3BhdGg+PC9nPjwvc3ZnPg==)Edit
this page

</div>

<div>

Last updated on **Feb 24, 2023** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>