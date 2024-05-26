# GitHub Cert Notes

## Tutorial Focus/Problems Solved

- How to create a simple way to write and generate podcast feats that are hostable using GitHub Pages
- Build actions locally, then publish them to the GitHub Marketplace
  - Creating a Podcast Feed Generator
  - Involves:
    - [XML, RSS Feeds](https://help.apple.com/itc/podcasts_connect/#/itcbaf351599)
    - [YAML Syntax](https://yaml.org/)
    - [GitHub Pages](https://pages.github.com/)
    - Python
    - Docker
    - Bash Scripting

- To add to developer portfolio:
  - Think about a problem you're having an how GitHub Actions could help solve it

## Prerequisites

- Git and GitHub
- Command Line
- Programming Experience

## Requirements

- Create own repo in order to follow along

## Course Info and Slides

- [Slides located here](https://raybo.org/slides_practicalactions)
- [Course GitHub](https://github.com/LinkedInLearning/github-practical-actions-4412872)
  - GitHub Course has branches associated with specific LinkedIn Learning movies from the course

Slides correspond to first three videos in Chapter 1

### Chapter 01: Action Basics

#### 01_01: GitHub Actions

- [Actions Information](https://github.com/features/actions)
- [Actions Marketplace](https://github.com/marketplace?category=&query=&type=actions)
- [Actions Documentation](https://docs.github.com/en/actions)

##### Notes

GitHub Actions help automate projects

- Can automate tasks for repos
  - Time Based
  - Event Based
- Can create own actions or use actions created by the community
  - Find these on GitHub Marketplace

Note: When trying to learn something, have a problem you're trying to solve

Podcast Feed:

- A way for podcast apps to find out info about a show and episodes
- Technical
  - An XML file in a specific format called RSS
    - XML: A markup language like HTML, but used for different kinds of info
    - RSS: A flavor of XML specifically for syndicating content
      - Usually used for news and podcast content

This course:

- Uses:
  - YAML format to write the info about the podcast
  - GitHub action to generate the feed
  - Python to parse YAML into XML
  - Dockerfile
  - Bash scripts
- XML and RSS are difficult to create, while YAML is easy to format and write
- Many services available to use to easily host podcast feed, but GitHub is a free solution for this problem
  - Use GitHub Pages
    - Allows hosting of static websites for free (e.g. webpages, podcast feed, etc.)

To publish this in marketplace, need a Dockerfile

- GitHub Actions uses this to build a container to run the GitHub action in the cloud
- Bash Scripts
  - Run the python file
  - Ensure changes are pushed back to GitHub

#### 01_02: Setting Up Your Repo

##### Notes

- Set up a Repo to take advantage of GitHub pages to host podcast feed
- Download [files as zip](https://github.com/LinkedInLearning/github-practical-actions-4412872/archive/refs/heads/01_02b.zip) from repo to get:
  - folders: audio, images
  - files: README.md, README-feed.md, feed.yaml


#### 01_03: Processing with Python

- [RSS Feed Sample](https://help.apple.com/itc/podcasts_connect/#/itcbaf351599)
- [Guide](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

##### Requirements

- [Python Installed](https://www.python.org/downloads/)
- [PyYAML](https://pypi.org/project/PyYAML/)
  - Helps parse YAML file

##### Basic Python Parsing Example

```py
import yaml # Import the PyYAML package
import xml.etree.ElementTree as xml_tree # Import the ElementTree module from the xml.etree package

# Load YAML data from file
with open('feed.yaml', 'r') as file:
    yaml_data = yaml.safe_load(file) # Use the safe_load() function to load the YAML data from the file

# Create RSS element with appropriate attributes and namespaces
rss_element = xml_tree.Element('rss', {'version': '2.0',
                                    'xmlns:itunes': 'http://www.itunes.com/dtds/podcast-1.0.dtd',
                                    'xmlns:content': 'http://purl.org/rss/1.0/modules/content/'})

channel_element = xml_tree.SubElement(rss_element, 'channel') # Create channel element as a child of the RSS element
link_prefix = yaml_data['link'] # Store the link prefix in a variable for convenience

# Add subelements to the channel element using data from the YAML file
xml_tree.SubElement(channel_element, 'link').text = link_prefix
xml_tree.SubElement(channel_element, 'format').text = yaml_data['format']
xml_tree.SubElement(channel_element, 'title').text = yaml_data['title']
xml_tree.SubElement(channel_element, 'subtitle').text = yaml_data['subtitle']
xml_tree.SubElement(channel_element, 'itunes:author').text = yaml_data['author']
xml_tree.SubElement(channel_element, 'description').text = yaml_data['description']
xml_tree.SubElement(channel_element, 'itunes:image', {'href': link_prefix + yaml_data['image']})
xml_tree.SubElement(channel_element, 'language').text = yaml_data['language']
xml_tree.SubElement(channel_element, 'itunes:explicit').text = yaml_data['explicit']
xml_tree.SubElement(channel_element, 'link').text = link_prefix

# Create an 'itunes:category' subelement within the 'channel' element, and set its 'text' attribute to the value of the 'category' field from the YAML data
category = xml_tree.SubElement(channel_element, 'itunes:category', {'text': yaml_data['category']})

# Loop through each item in the YAML file's 'item' section, and add an 'item' subelement to the channel element for each one
for item in yaml_data['item']:
    item_element = xml_tree.SubElement(channel_element, 'item')
    xml_tree.SubElement(item_element, 'title').text = item['title']
    xml_tree.SubElement(item_element, 'itunes:author').text = yaml_data['author']
    xml_tree.SubElement(item_element, 'description').text = item['description']
    xml_tree.SubElement(item_element, 'itunes:duration').text = item['duration']
    xml_tree.SubElement(item_element, 'pubDate').text = item['published']
    
    # Add an 'enclosure' subelement to the 'item' element, 
    # which specifies the audio file associated with this item
    enclosure = xml_tree.SubElement(item_element, 'enclosure', {
        'url': link_prefix + item['file'],
        'type': 'audio/mpeg',
        'length': item['length']
    })

# Write the created XML tree to a file
output_tree = xml_tree.ElementTree(rss_element) # Create an ElementTree object from the RSS element
output_tree.write('podcast.xml', encoding='UTF-8', xml_declaration=True) # Write the XML tree to a file
```

#### 01_04: Finishing up the RSS feed

### Chapter 02: Publishing a Marketplace Action

#### 02_01: Creating a workflow with existing actions

#### 02_02: Creating a generator repo Dockerfile

#### 02_03: Creating an entry point

#### 02_04: Crafting an action.yml file

#### 02_05: Testing your actions

#### 02_06: Creating a release

### Suggestions for Further Learning
