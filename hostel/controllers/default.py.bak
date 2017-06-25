# -*- coding: utf-8 -*-
# this file is released under public domain and you can use without limitations

#########################################################################
## This is a sample controller
## - index is the default action of any application
## - user is required for authentication and authorization
## - download is for downloading files uploaded in the db (does streaming)
#########################################################################

@auth.requires_login()
def login():
    if auth.has_membership('moderator'):
        redirect(URL('default','moderate'))
    if auth.has_membership('solver'):
        redirect(URL('default','solve'))
    query = (db.auth_event.description.like('%Logged-in%'))&(db.auth_event.user_id==auth.user.id) 
    if db(query).count() == 1:
        redirect(URL('default','profile'))
    else :
        redirect(URL('default','home'))
    return
@auth.requires(login)
def index():
    """
    example action using the internationalization operator T and flash
    rendered by views/default/index.html or views/generic.html

    if you need a simple wiki simply replace the two lines below with:
    return auth.wiki()
    """
    response.flash = T("Hello")
    return dict(message=T('Welcome to hostel management portal!'))

def home():
    response.flash = T("Hello")
    return dict(message=T('Welcome to hostel management portal!'))

def profile():
    register=auth.register()
    return dict(register = register)

def electricals():
    db.electrical.status.writable=False
    db.electrical.status.readable=False
    db.electrical.progress.writable=False
    db.electrical.progress.readable=False
    form=SQLFORM(db.electrical).process()
    return dict(form=form)

def viewelectrical():
    rows=db(db.electrical.iden==auth.user.id).select(orderby=~db.electrical.created_on)
    return locals()

def viewwater():
    rows=db(db.water.iden==auth.user.id).select(orderby=~db.water.created_on)
    return locals()

def viewwash():
    rows=db(db.washingmachine.iden==auth.user.id).select(orderby=~db.washingmachine.created_on)
    return locals()

def viewother():
    rows=db(db.other.iden==auth.user.id).select(orderby=~db.other.created_on)
    return locals()

def waters():
    db.water.status.writable=False
    db.water.status.readable=False
    db.water.progress.writable=False
    db.water.progress.readable=False
    form=SQLFORM(db.water).process()
    return dict(form=form)
def washingmachines():
    db.washingmachine.status.writable=False
    db.washingmachine.status.readable=False
    db.washingmachine.progress.writable=False
    db.washingmachine.progress.readable=False
    form=SQLFORM(db.washingmachine).process()
    return locals()
def others():
    db.other.status.writable=False
    db.other.status.readable=False
    db.other.progress.writable=False
    db.other.progress.readable=False
    form=SQLFORM(db.other).process()
    return locals()
@auth.requires(login)
def problem():
    return locals()
@auth.requires(login)
def viewproblem():
    return locals()

@auth.requires_membership("moderator")
def moderate():
    return locals()


def options_widget(field,value,**kwargs):
    """ Use web2py's intelligence to set up the right HTML for the select field
     the widgets knows about the database model """
    w = SQLFORM.widgets.options.widget
    xml = w(field,value,**kwargs)
    return xml

def options_widget(field,value,**kwargs):
    return SQLFORM.widgets.options.widget(field,value,**kwargs)
"""
def editable_grid():
    #process submitted form
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():   
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.electrical.id==row_id).update(**{field_name:value})
    db.electrical.status.represent = lambda value,row:  options_widget(db.electrical.status,value,
                     **{'_name':'status_row_%s' % row.id})
    grid = SQLFORM.grid(db.electrical.status,orderby=~db.electrical.created_on,create=False,details=False,editable=False,selectable= lambda ids : redirect(URL('default','editable_grid',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)
"""
@auth.requires_membership("moderator")
def moderate_electrical():
    #process submitted form
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():   
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.electrical.id==row_id).update(**{field_name:value})
    db.electrical.status.represent = lambda value,row:  options_widget(db.electrical.status,value,
                     **{'_name':'status_row_%s' % row.id})
    grid = SQLFORM.grid(db.electrical.status!='approved',orderby=~db.electrical.created_on,create=False,editable=False,selectable= lambda ids : redirect(URL('default','moderate_electrical',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)
@auth.requires_membership("moderator")
def moderate_plumbing():
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():   
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.water.id==row_id).update(**{field_name:value})
    db.water.status.represent = lambda value,row:  options_widget(db.water.status,value,
                     **{'_name':'status_row_%s' % row.id})
    grid = SQLFORM.grid(db.water.status!='approved',orderby=~db.water.created_on,create=False,editable=False,selectable= lambda ids : redirect(URL('default','moderate_plumbing',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)

@auth.requires_membership("moderator")
def moderate_washingmachine():
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():   
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.washingmachine.id==row_id).update(**{field_name:value})
    db.washingmachine.status.represent = lambda value,row:  options_widget(db.washingmachine.status,value,
                     **{'_name':'status_row_%s' % row.id})
    grid = SQLFORM.grid(db.washingmachine.status!='approved',orderby=~db.washingmachine.created_on,create=False,editable=False,selectable= lambda ids : redirect(URL('default','moderate_washingmachine',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)
@auth.requires_membership("moderator")
def moderate_others():
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():   
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.other.id==row_id).update(**{field_name:value})
    db.other.status.represent = lambda value,row:  options_widget(db.other.status,value,
                     **{'_name':'status_row_%s' % row.id})
    grid = SQLFORM.grid(db.other.status!='approved',orderby=~db.other.created_on,create=False,editable=False,selectable= lambda ids : redirect(URL('default','moderate_others',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)


def solve():
    return locals()
"""
def display():
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.electrical).update(**{field_name:value})
    db.electrical.progress.represent = lambda value,row:  options_widget(db.electrical.progress,value,
                     **{'_name':'progress_row_%s' % row.id})
    grid = SQLFORM.grid(((db.electrical.status=='approved')&(db.electrical.progress!='solved')),orderby=~db.electrical.created_on,create=False,details=False,editable=False,deletable=False,selectable= lambda ids : redirect(URL('default','display',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)
"""
@auth.requires_membership("solver")

def solve_electrical():
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.electrical.id==row_id).update(**{field_name:value})
    db.electrical.progress.represent = lambda value,row:  options_widget(db.electrical.progress,value,
                     **{'_name':'progress_row_%s' % row.id})
    grid = SQLFORM.grid(((db.electrical.status=='approved')&(db.electrical.progress!='solved')),orderby=~db.electrical.created_on,create=False,editable=False,deletable=False,selectable= lambda ids : redirect(URL('default','solve_electrical',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)


def solve_plumbing():
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.water.id==row_id).update(**{field_name:value})
    db.water.progress.represent = lambda value,row:  options_widget(db.water.progress,value,
                     **{'_name':'progress_row_%s' % row.id})
    grid = SQLFORM.grid(((db.water.status=='approved')&(db.water.progress!='solved')),orderby=~db.water.created_on,create=False,editable=False,deletable=False,selectable= lambda ids : redirect(URL('default','solve_plumbing',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)





def solve_washingmachine():
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.washingmachine.id==row_id).update(**{field_name:value})
    db.washingmachine.progress.represent = lambda value,row:  options_widget(db.washingmachine.progress,value,
                     **{'_name':'progress_row_%s' % row.id})
    grid = SQLFORM.grid(((db.washingmachine.status=='approved')&(db.washingmachine.progress!='solved')),orderby=~db.washingmachine.created_on,create=False,editable=False,deletable=False,selectable= lambda ids : redirect(URL('default','solve_washingmachine',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)



def solve_others():
    if len(request.post_vars) > 0:
        for key, value in request.post_vars.iteritems():
            (field_name,sep,row_id) = key.partition('_row_') #name looks like home_state_row_99
            if row_id:
                db(db.other.id==row_id).update(**{field_name:value})
    db.other.progress.represent = lambda value,row:  options_widget(db.other.progress,value,
                     **{'_name':'progress_row_%s' % row.id})
    grid = SQLFORM.grid(((db.other.status=='approved')&(db.other.progress!='solved')),orderby=~db.other.created_on,create=False,editable=False,deletable=False,selectable= lambda ids : redirect(URL('default','solve_others',vars=request._get_vars)))
    grid.elements(_type='checkbox',_name='records',replace=None)
    return dict(grid=grid)





def user():
    """
    exposes:
    http://..../[app]/default/user/login
    http://..../[app]/default/user/logout
    http://..../[app]/default/user/register
    http://..../[app]/default/user/profile
    http://..../[app]/default/user/retrieve_password
    http://..../[app]/default/user/change_password
    http://..../[app]/default/user/bulk_register
    use @auth.requires_login()
        @auth.requires_membership('group name')
        @auth.requires_permission('read','table name',record_id)
    to decorate functions that need access control
    also notice there is http://..../[app]/appadmin/manage/auth to allow administrator to manage users
    """
    return dict(form=auth())


@cache.action()
def download():
    """
    allows downloading of uploaded files
    http://..../[app]/default/download/[filename]
    """
    return response.download(request, db)

def info():
    return locals()

def call():
    """
    exposes services. for example:
    http://..../[app]/default/call/jsonrpc
    decorate with @services.jsonrpc the functions to expose
    supports xml, json, xmlrpc, jsonrpc, amfrpc, rss, csv
    """
    return service()
