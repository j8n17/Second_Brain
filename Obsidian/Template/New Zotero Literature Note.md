---
type:
tags: zotero, note/zotero
created:
updated:
---
## Self notes
**Research**
- Methodology::
- Key Constructs::
	- IVs::
	- DVs::
	- Moderators::
	- Others::
- Key_Findings::
- Contributions::
- Limitations::

**Self Critique**
- Critique
	- Pros
	- Cons
- How is it relevant to my research?
	- Relevant_topic::
	- Use::


> [!Cite]  
> {{bibliography}}

>[!Synth]  
>**Contribution**::

>[!md]  
> {%- for creator in creators %} {%- if creator.name == null %} **{{creator.creatorType | capitalize}}**:: {{creator.lastName}}, {{creator.firstName}}{%- endif -%}<br>  
> {%- if creator.name %}**{{creator.creatorType | capitalize}}**:: {{creator.name}}{%- endif -%}{%- endfor %}  
> **Title**:: {{title}}  
> **Year**:: {{date | format("YYYY")}}  
> **Citekey**:: @{{citekey}}  
> > {%- if allTags %}**Tags**:: {{allTags}} {%- endif %}  
> {%- if itemType %}**itemType**:: {{itemType}}{%- endif %}  
> {%- if itemType == "journalArticle" %}**Journal**:: *{{publicationTitle}}* {%- endif %}  
> {%- if volume %}**Volume**:: {{volume}} {%- endif %}  
> {%- if issue %}**Issue**:: {{issue}} {%- endif %}  
> {%- if itemType == "bookSection" %}**Book**:: {{publicationTitle}} {%- endif %}  
> {%- if publisher %}**Publisher**:: {{publisher}} {%- endif %}  
> {%- if place %}**Location**:: {{place}} {%- endif %}  
> {%- if pages %} **Pages**:: {{pages}} {%- endif %}  
> {%- if DOI %}**DOI**:: {{DOI}} {%- endif %}  
> {%- if ISBN %}**ISBN**:: {{ISBN}} {%- endif %}

> [!LINK]  
> {%- for attachment in attachments | filterby("path", "endswith", ".pdf") %}  
> [{{attachment.title}}](file://{{attachment.path | replace(" ", "%20")}}) {%- endfor -%}.

> [!Abstract]  
> {%- if abstractNote %}  
> abstract:: {{abstractNote}}  
> {%- endif -%}
> 

---

## Annotations 
{% for annotation in annotations -%}{%- if annotation.annotatedText -%}{% if 'Gray' in annotation.colorCategory %} 
##### {{annotation.annotatedText | escape }}{% else %} 
> [!Highlight]
> <mark class="customZot-{% if annotation.color %}{{annotation.colorCategory}} {% endif %}">{{annotation.annotatedText | escape }}</mark> ([{{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}))
> {% endif %} {%- endif %} {% if annotation.imageRelativePath %} 
> [!Image]
> <mark class="customZot-{% if annotation.color %}{{annotation.colorCategory}} {% endif %}"> Image</mark>
> ![[{{annotation.imageRelativePath}}]] 
> {% endif %}{% if annotation.comment %}
> **Comment**:
> 
> *{{annotation.comment}}* {% endif %}
{% endfor -%}


---
