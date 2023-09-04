# copyrepo

name: Move Files

on:
  push:
    branches:
      - qa

jobs:
  move-files:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Delete Existing Files in prod Folder
        run: |
          rm -rf prod/*
          
      - name: Move Files from dev to prod
        run: |
          mv dev/* prod/

      - name: Set Git Identity
        run: |
          git config --global user.email "dchekuru@gmail.com"
          git config --global user.name "DineshChekuru"

      - name: Pull Latest Changes
        run: git pull origin qa

          
      # - name: Commit and Push
      #   run: |
      #     git status
      #     git add .
      #     git commit -am "Move files from folder1 to folder2" || echo "No changes to commit"
      #     git remote set-url origin "https://$GH_PAT@github.com/vinodkumar704/copyrepo.git"
      #     git push origin qa
      #   env:
      #     GH_PAT: ${{ secrets.GH_PAT }}

      - name: Commit and Push
        run: |
          git --version
          git remote -v
          #git remote rm copyrepo
          #git remote add copyrepo https://github.com/vinodkumar704/copyrepo
          git remote -v  # Check if 'copyrepo' is listed
          git status
          git add .
          git commit -m "Move files from folder1 to folder2" 
          git remote add copyrepo https://github.com/vinodkumar704/copyrepo
          git push https://USERNAME:${{ secrets.QAAUTH }}@github.com/vinodkumar704/copyrepo HEAD:qa
          GIT_TRACE=1 git push copyrepo
        env:
          GITHUB_TOKEN: ${{ secrets.QAAUTH }}


      
        
        
