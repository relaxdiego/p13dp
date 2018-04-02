.PHONY: create configure destroy
.DEFAULT: create

create: .make-import
	@hooks/lib/make-create
	@touch .make-recreate

pre-configure: .make-recreate

destroy:
	@hooks/lib/make-destroy
	@rm -f .make-recreate

.make-recreate: .make-import
	@hooks/lib/make-destroy
	@hooks/lib/make-create
	@touch .make-recreate

.make-import: ${packer_output_path}/${project_name}-${os_distribution}-${provider}/*.box
	@echo "Box was modified since the last time it was imported"
	@hooks/lib/make-import
	@touch .make-import
