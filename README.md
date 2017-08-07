# SlideMenu
Add This File to your Project 

RECommonFunctions.h
RECommonFunctions.m
REFrostedContainerViewController.h
REFrostedContainerViewController.m
REFrostedViewController.h
REFrostedViewController.m
UIImage+REFrostedViewController.h
UIImage+REFrostedViewController.m
UIView+REFrostedViewController.h
UIView+REFrostedViewController.m
UIViewController+REFrostedViewController.h
UIViewController+REFrostedViewController.m


// Add below method to AppDelegate.h
- (void)changeRootViewController:(UIViewController*)viewController;


// Add on AppDelegate.m for change root ViewController

- (void)changeRootViewController:(UIViewController*)viewController {
    
    if (!self.window.rootViewController) {
        self.window.rootViewController = viewController;
        return;
    }
    UIView *overlayView = [[UIScreen mainScreen] snapshotViewAfterScreenUpdates:NO];
    [viewController.view addSubview:overlayView];
    self.window.rootViewController = viewController;
    
    [UIView animateWithDuration:0.4f delay:0.0f options:UIViewAnimationOptionTransitionCrossDissolve animations:^{
        overlayView.alpha = 0;
    } completion:^(BOOL finished) {
        [overlayView removeFromSuperview];
    }];
    
    self.window.rootViewController = viewController;
}





// For Register or configure REFrostedViewController as a root view Controller 
// On LoginView Controller 
-(void)showHomeViewController {
    
    AppDelegate *appDel = [[UIApplication sharedApplication] delegate];
    
    UIViewController *homeVC = [self.storyboard instantiateViewControllerWithIdentifier:@"HomeViewController"];
    
    UINavigationController *navigationController = [[UINavigationController alloc]initWithRootViewController:homeVC];
    
    // MenuViewController For to Slidemenu Item
    MenuViewController *menuController = [self.storyboard instantiateViewControllerWithIdentifier:@"MenuViewController"];
    
    //Create frosted view controller
    REFrostedViewController *frostedViewController = [[REFrostedViewController alloc] initWithContentViewController:navigationController menuViewController:menuController];
    
    frostedViewController.direction = REFrostedViewControllerDirectionLeft;
    frostedViewController.liveBlurBackgroundStyle = REFrostedViewControllerLiveBackgroundStyleLight;
    frostedViewController.liveBlur = YES;
    frostedViewController.delegate = self;
    [appDel changeRootViewController:frostedViewController];
    
}




- (IBAction)LoginButtonAction:(id)sender {
    
    [self showHomeViewController];
}

