% Understanding Code
% Ravikiran K.S.
% January 1, 2006


For what it's worth, I had a professional reputation (with my
management) as being good at that, and I did a lot of support
programming fixing other people's code.

But it's hard to come up with tips, because they depend on the code
you're reading. Here are a few ideas:

(1) Read the comments. If the references are obscure, don't kill
yourself (or your available time) trying to understand them, but pick up
any clues that are readily available.

(2) Don't trust the comments. They might not have been accurate when
written, and they also might not have been maintained when the code was
revised.

(3) Look for interfaces: input, output, parameters, returned values. See
what's actually produced.

(4) Look for side effects. Sometimes they're the point. (I wish I could
say that the comments always explain that, but rule (2) prohibits me
from doing so.)

(5) Try to get a “feel” for what the variables represent in relation to
the code's design. Again, maybe there are comments to help–and again,
maybe they can't be trusted.

One of the barriers to reading someone else's code is learning to accept
someone else's variable names. It's like doing a cryptogram: you have to
get used to the fact that, for this particular case, L represents what
you'd write as T, and so on. You're trying to think someone else's
thoughts instead of your own.

(6) Look for structure; that is, mentally divide the code into sections
and try to get a grasp of what each does. Well-written code should
naturally lend itself to this. Some code doesn't.

(7) Withhold judgment on things that are designed differently from the
way you'd do them, at least until you are sure exactly what they do. See
source for a relevant quote.

(8) Always be on the lookout for something (a technique, a style, an
algorithm) you can learn from the code you're reading. It's not always
there, but looking for it can help your understanding anyway.

(9) Use scratch paper. Better yet, print out the code and mark up your
copy as needed to clarify structure. I frequently used highlighting in
multiple colors as an analysis tool.

Source(s): A yielding, an obedience, a willingness to accept these notes
as the right notes, this pattern as the true pattern, is the essential
gesture of performance, translation, and understanding. The gesture need
not be permanent, a lasting posture of the mind or heart; yet it is not
false. It is more than the suspension of disbelief needed to watch a
play, yet less than a conversion. It is a position, a posture in the
dance. – Ursula K. Le Guin, “The Telling”

