# Research Log Anton

## 6.6.23
### Idea behind the project:
 - Collecting good groundtruth takes a lot of time
 - This applies to data used for training and evaluation but also for any HIL setting where the user needs to manually label data
 - Meta's recent paper on Segment-Anything (SAM) brings a new possibility for labelling imagery on scale with prompts
 - The prompts can be either text prompts or pickers

### Goal:
 - I want to show the feasibility of applying SAM to a typical aerial/satellite imagery use case: Detecting bridges
 - If everything goes super-smooth with pre-existing tooling, I plan to extend the solution horizontally (e.g., dataset management)
 - If it doesn't work out straight out of the box, I will have an opportunity to make it work

### Milestones:
 - First Goal: Evaluate the existing solution by https://github.com/opengeos/segment-geospatial
   - 18:47: I tried to use the existing code examples out of the box, applying it to a bridge in Munich. Neither the text prompt "bridge", nor manually set pickers worked out. Looking into the issue in more detail, it turned out that the generated masks are not even suggesting the bridge as an object of interest. Overall, the objects that were detected are not very relevant and quite arbitrary. I'll need to check whether this might be a data formatting or tooling usage error or just a plain model weakness. Next, I'll try to reproduce exactly what the opengeos reference implementation is doing. If that yields the same results as in the documentation, I will try to detect the golden gate bridge using my techniques.
   - 19:40: I was able to get the same result as they did for their trees. But also for them it didn't work properly


## 7.6.23
- After yesterday's limited results from the SAM model for bridges, I decided to approach the problem from another direction: By using OSM data
- OpenStreetMap (OSM) supports an easy query language to access labels. I used this to query all bridges within Munich and saved them as a geojson file
- I was able to verify with Esri Satellite imagery that the bridge masks are correct
- Next, I can use this data either to fine-tune the SAM model or to just train a new model only on the OSM data, depending on the research direction
  - Using the data to finetune SAM would help to evaluate SAM finetuning capabilities. I could look into few-shot learning and analyze how many data points are necessary for getting a model that detects 90% of the OSM bridges
  - I can then use the same data and evaluation for testing alternative solutions like finetuning an existing imagenet trained model or an existing aerial imagery model
  - The goal of this experiment is to find out whether SAM is providing any real benefit for satellite imagery collection campaigns
