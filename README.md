# Blogs

* [Home](https://thobanimadonsela.github.io/blogs/)


# Python

* [Resources](https://github.com/thobanimadonsela/blogs/wiki/Python-Reads)


# Tracing

## Jaeger with Docker Locally

- Download Jaeger: Pull the Jaeger Docker image from the official repo:

```
docker pull jaegertracing/all-in-one:latest
```

- Run Jaeger: Launch Jaeger using Docker:

```
 docker run -d --name jaeger -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 -p 5775:5775/udp -p 6831:6831/udp -p 6832:6832/udp -p 5778:5778 -p 16686:16686 -p 14268:14268 -p 14250:14250 -p 9411:9411 jaegertracing/all-in-one:latest
```

## Example

```python
def attach_debug_trace(context):
    attach_debug_trace = context.get_settings().get("attach_debug_trace", False)
    if attach_debug_trace:
        # Configure the tracer provider
        trace.set_tracer_provider(TracerProvider(resource=Resource.create({SERVICE_NAME: context.title})))

        # Configure the Jaeger exporter
        jaeger_exporter = JaegerExporter(
            agent_host_name='localhost', 
            agent_port=6831, 
            collector_endpoint="http://localhost:14268/api/traces?format=jaeger.thrift")

        # Create a span processor and add the exporter to it
        span_processor = BatchSpanProcessor(jaeger_exporter)
        trace.get_tracer_provider().add_span_processor(span_processor)

```

## View Traces
- http://localhost:16686/search
