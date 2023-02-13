# Shards PBR Demo

This demo contains some reusable code to add simple image based lighting to any project.
Rendering code is based on the [glTF sample viewer](https://github.khronos.org/glTF-Sample-Viewer-Release/) ([source](https://github.com/KhronosGroup/glTF-Sample-Viewer))
Ported logic of the [IBL sampler](https://github.com/KhronosGroup/glTF-IBL-Sampler) that the glTF viewer uses is also included to allow generation of environment probes on the fly

## Shared setup

```clojure
(setup-shared-sky-sphere-queue)
```
Needs to be ran before using any other functionality. This is required for the cubemap rendering utilities.

## Procedural skybox

A ported version of [Fewes/MinimalAtmosphere](https://github.com/Fewes/MinimalAtmosphere) to render a procedural sky

```clojure
(setup-skybox-pass) >= .skybox-pass
```

The `setup-skybox-pass` generates a `DrawablePass` that renders the sky on top of an existing scene (only rendering where depth buffer is 1.0). So place this drawable pass after your other render passes.

It takes 2 variables from the global scope

```clojure
(Float3 -6.0 10.0 2.5) (Math.Normalize) = .light-direction
(Float3 1 1 1) =  .light-color
```

## Environment lighting

The following adds IBL based on one static scene probe:

```clojure
(setup-pbr-feature .env-included-steps (Float3 0.0 1.0 0.0)) >> .features
```

The first argument is a sequence of drawable steps that are rendered into the probe (the skybox pass would be good to insert here)

The second argument is a probe location offset.
