# auto merge ok done 3.

name: Auto Merge

on:
  push:
    branches:
      - pre-production

env:
  MY_REPO: https://sonali-makerble:${{secrets.GITHUB_TOKEN}}@github.com/sonali-makerble/auto-merge-to-develop.git
  MY_BRANCH: develop
  MASTER_REPO: https://sonali-makerble:${{secrets.GITHUB_TOKEN}}@github.com/sonali-makerble/auto-merge-to-develop.git
  MASTER_BRANCH: pre-production

jobs:
  merge:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repo
      uses: actions/checkout@v2
      with:
        ref: ${{env.MASTER_BRANCH}}
        token: ${{ secrets.GITHUB_TOKEN }}


    # - name: Merge pre-production into develop
    #   run: |
    #     git clone ${{env.MASTER_REPO}} -b ${{env.TO_BRANCH}} tmp
    #     cd tmp
    #     git config user.name "Automerge Bot"
    #     git config user.email "automergebot@makerble.com"
    #     git config pull.rebase false
    #     git pull ${{env.MASTER_REPO}} ${{env.FROM_BRANCH}}
    #     git push origin ${{env.TO_BRANCH}}

    - name: Create new branch
      run: |
        git config user.name "Automerge Bot"
        git config user.email "bot@example.com"
        git checkout -b merge-${{env.MASTER_BRANCH}}-to-${{env.MY_BRANCH}}
        git push origin merge-${{env.MASTER_BRANCH}}-to-${{env.MY_BRANCH}}

    - name: Create Pull Request
      id: cpr
      uses: peter-evans/create-pull-request@v5
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
        commit-message: merge changes from pre-procution to develop branch
        committer: GitHub <noreply@github.com>
        author: ${{ github.actor }} <${{ github.actor }}@users.noreply.github.com>
        signoff: false
        branch: merge-${{env.MASTER_BRANCH}}-to-${{env.MY_BRANCH}}
        delete-branch: true
        body: "This PR is automatically created to merge changes from ${MASTER_BRANCH} into ${MY_BRANCH}."


-------------------------

name: Auto Merge

on:
  push:
    branches:
      - pre-production

env:
  MY_REPO: https://sonali-makerble:${{secrets.GITHUB_TOKEN}}@github.com/sonali-makerble/auto-merge-to-develop.git
  MY_BRANCH: develop
  MASTER_REPO: https://sonali-makerble:${{secrets.GITHUB_TOKEN}}@github.com/sonali-makerble/auto-merge-to-develop.git
  MASTER_BRANCH: pre-production

jobs:
  merge:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repo
      uses: actions/checkout@v2
      with:
        ref: ${{env.MASTER_BRANCH}}
        token: ${{ secrets.GITHUB_TOKEN }}


    # - name: Merge pre-production into develop
    #   run: |
    #     git clone ${{env.MASTER_REPO}} -b ${{env.TO_BRANCH}} tmp
    #     cd tmp
    #     git config user.name "Automerge Bot"
    #     git config user.email "automergebot@makerble.com"
    #     git config pull.rebase false
    #     git pull ${{env.MASTER_REPO}} ${{env.FROM_BRANCH}}
    #     git push origin ${{env.TO_BRANCH}}

    - name: Create new branch
      run: |
        git config user.name "Automerge Bot"
        git config user.email "bot@example.com"
        git checkout -b merge-${{env.MASTER_BRANCH}}-to-${{env.MY_BRANCH}}
        git push origin merge-${{env.MASTER_BRANCH}}-to-${{env.MY_BRANCH}}

    - name: Raise PR 1
      id: cpr
      uses: peter-evans/create-pull-request@v3
      with:
        branch: merge-${{env.MASTER_BRANCH}}-to-${{env.MY_BRANCH}}
        base:  ${{env.MY_BRANCH}}
        title: "demo for auto pr1"
        committer: sonali <sonali123@gmail.com>
        author: sonali <sonali123@gmail.com>
        body:
          This is to show automatic PR creation
        token: ${{ secrets.GITHUB_TOKEN }}
        

    - name: Approve Pull Request1
      uses: juliangruber/approve-pull-request-action@v1
      with:
        github-token: ${{ secrets.GITHUB_TOKEN}}
        number: ${{ steps.cpr.outputs.pull-request-number }}

    - name: Merge Pull request1
      uses: juliangruber/merge-pull-request-action@v1
      with:
        github-token: ${{ secrets.GITHUB_TOKEN }}
        number: ${{ steps.cpr.outputs.pull-request-number }}
        method: squash
