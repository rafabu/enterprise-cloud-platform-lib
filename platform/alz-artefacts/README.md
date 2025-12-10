# Add-on ALZ library Artefacts

## Terraform Templatefile support

Module `az-alz-base` supports template rendering on those artefacts. Make sure that:

- file name matches [artefact-name]*.tftemplate*[.json]

Files not following above pattern will not be rendered and fed as-is to the alz provider.
