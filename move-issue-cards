/**
 * This is a template for an on-change rule. This rule defines what
 * happens when a change is applied to an issue.
 *
 * For details, read the Quick Start Guide:
 * https://www.jetbrains.com/help/youtrack/devportal/Quick-Start-Guide-Workflows-JS.html
 */

const entities = require('@jetbrains/youtrack-scripting-api/entities');

exports.rule = entities.Issue.onChange({
  title: 'Move-issues',
  
  guard: (ctx) => {	
    const pr =  ctx.issue.pullRequests.added.first()
    
    if (!pr) return false;
              
    const newPr = pr.state.name == "OPEN" && ctx.issue.fields.State.name == ctx.State.InProgress.name;
    const mergedPr = pr.state.name == "MERGED" && ctx.issue.fields.State.name == ctx.State.InReview.name;
    
    return newPr || mergedPr
  },
  
  action: (ctx) => {    
    const newPr = ctx.issue.fields.State.name == ctx.State.InProgress.name;
    const mergedPr = ctx.issue.fields.State.name == ctx.State.InReview.name;
    
    if (newPr) {
    	ctx.issue.fields.State = ctx.State.InReview;    
    }
    else if (mergedPr) {
    	ctx.issue.fields.State = ctx.State.Done;      
    }    
  },
  
  requirements: {
    State: {
  		type: entities.State.fieldType,
  		InProgress: {
      		name: "In Progress"
    	},
        InReview: {
          name: "In Review"          
        },
  		Done: {},  		
	},  
        
  }  
});
