## @sfra

#### Temporary development solution to use Verdaccio as local npm repo
`npm config set @sfra:registry http://localhost:4873`

#### This project is a Monorepo (work in progress)

This means the sfra core, extensions and reference application are all managed, tested and versioned together in this single repository.
Packages which include core, services and extensions will be published and consumed as npm modules individually, but will be developed and managed under this single developer repository.  

#### Dependencies

The `packages` folder contains individual npm projects. 
Many will share common dependencies and lerna will be used to `HOIST` these
dependencies to the top level `node_modules` folder rather than each package
creating a copy of the dependencies. 

#### Setup

1) `npm install --global lerna` 
2) In this folder run `lerna bootstrap --hoist` (rather than npm install)
3) To build all the `packages` run `lerna run build --stream`
4) To `build` and `publish` and packages run `lerna run pub --stream` 
5) To add a shared dependency to a package use the `lerna add dep1@0.0.1 --scope=package1`. Use `--hoist` if the dependency is to be shared by more than one package.

#### Lerna notes

1) lerna should be used to add and remove dependencies. Avoid npm update/install/add commands in child packages if possible.
2) Upgrade a single package dependency (no share no hoist)`lerna add dep1@0.0.2 --scope=package1`
3) When using `--hoist` with lerna child packages `node_modules` folders will have symbolic links created to the project root `node_modules/[dependency]` saving disk space also.
4) Always strive to same versioned/shared dependencies to reduce duplicate disk files but also to avoid large bundling of two different versions of something which could be shared.
 