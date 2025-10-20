
Hi all,

@Francisco Coelho Regarding our meeting this morning, I think I've found the bug. As suspected it was related to 0/1-indexing. The maths in the paper assumes group indices from 1 to 2n(n-1), and Python uses 0-indexing. Therefore, for signatures that used endpoints we'd get an out of bounds error. E.g., for array [[1, 1], [1, 1]].

Mostly, the fixing consisted on: signature[index-1] += 1  # 0-based index

Given that, I believe the signature presented on the paper for "Fig. 2. One active couple in a 4 × 4 grid." is not entirely correct. On the paper we have [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0]
and, on my implementation (see attached) we get [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0].

I think I'm doing things right since the signature for "Fig. 3. Multiple active couples in a 10 × 10 grid." matches between the paper and my implementation.

@Hamid Shahbazkia Could you, please, confirm that the signature I computed is correct and, therefore, the one on the paper is not?

I send both the n×n and m×n cases. I think it's best to generalize for the new paper.

Thanks,
António

Hamid Shahbazkia <shahbazkia@gmail.com> escreveu (quinta, 23/01/2025 à(s) 19:31):


    Hi
    Let me add some general points and background.
    25 years ago I needed to integrate affine invariance into a neural network and at that time it was out of question to use all pairs combinations to acheive shift invariance  so I came up with this algorithm.

    After all these years we think it can be used again as data augmentation becomes again useful.

    All was done for outline of a shape or a volume. At that time it meant a black and white image. If we manage to validate the 3d signature I think we can apply it to color images  2d will be the position and the 3rd can be used as color. 
    This will preserve the structure of a color image in any affine transformation.
    Hope this clarifies a bit the subject.

    On Thursday, January 23, 2025, António Anjos <aanjos@uevora.pt> wrote:

        Hi Francisco,

        I'm adding the author of the idea in CC so he can follow the developments.

        Regarding the questions you raised in the notebook:

            "similar couple of points": These are two or more pairs of pixels with the same slope and length. That is, each pair defines a line segment with a specific slope and length.
            "same mathematical properties": This refers to the slope and length, in this case.
            "signature of image": This is a value we calculate from the image that remains invariant under geometric transformations (translation, rotation, etc.).
            "ExtractSignature in ExtractSignature-FS": This seems to be a bug resulting from the attempt to merge Algorithms III.9 and III.10 from the original paper (2D) to make things more adaptable to 3D. The parts dedicated to 3D were never seriously reviewed. This needs to be addressed.
            "ExtractSignature-FS returns a matrix": No, it returns a vector with the counts of the similar pairs, just like the others.

        I revisited the first sections of the paper to refresh my memory, and there are aspects that could be made clearer. There are also a few typos. For publication, we'll need to rewrite the paper to make it easier to read (something I had promised myself I wouldn’t do again :D). Some things were already modified, but I can't find the latest version. Damn!

        Since you have a background in mathematics, there are terms/definitions that you can probably correct/improve. Feel free to make suggestions.

        The formulas in the paper are defined for images with the first pixel at (0,0). If you proceed with Julia for your tests, keep this in mind.

        As I'm not currently inclined toward Julia, I've implemented ExtractSignature-T in Python (attached). In the implementation, I took the opportunity to generalize it for m×n images (in the paper, everything is defined for n×n). I don’t think this will complicate things too much in practical terms. If it does, we can stick to n×n.

        I also made an example on paper (attached). In the top column, you can see the possible lengths in a 3×3 image. Below each possible length, you can see the possible slopes, and all cases for those respective slopes.

        Hopefully, this will give you a good "jumpstart". In any case, we can talk tomorrow (morning?), if you want. In case you do, please let me know.

        Cheers,
        António



    -- 
    Hamid Reza Shahbazkia



---


Hi Francisco,

I'm adding the author of the idea in CC so he can follow the developments.

Regarding the questions you raised in the notebook:

- "similar couple of points": These are two or more pairs of pixels with the same slope and length. That is, each pair defines a line segment with a specific slope and length.

- "same mathematical properties": This refers to the slope and length, in this case.

- "signature of image": This is a value we calculate from the image that remains invariant under geometric transformations (translation, rotation, etc.).

- "ExtractSignature in ExtractSignature-FS": This seems to be a bug resulting from the attempt to merge Algorithms III.9 and III.10 from the original paper (2D) to make things more adaptable to 3D. The parts dedicated to 3D were never seriously reviewed. This needs to be addressed.

- "ExtractSignature-FS returns a matrix": No, it returns a vector with the counts of the similar pairs, just like the others.

I revisited the first sections of the paper to refresh my memory, and there are aspects that could be made clearer. There are also a few typos. For publication, we'll need to rewrite the paper to make it easier to read (something I had promised myself I wouldn’t do again :D). Some things were already modified, but I can't find the latest version. Damn!

Since you have a background in mathematics, there are terms/definitions that you can probably correct/improve. Feel free to make suggestions.

The formulas in the paper are defined for images with the first pixel at (0,0). If you proceed with Julia for your tests, keep this in mind.

As I'm not currently inclined toward Julia, I've implemented ExtractSignature-T in Python (attached). In the implementation, I took the opportunity to generalize it for m×n images (in the paper, everything is defined for n×n). I don’t think this will complicate things too much in practical terms. If it does, we can stick to n×n.

I also made an example on paper (attached). In the top column, you can see the possible lengths in a 3×3 image. Below each possible length, you can see the possible slopes, and all cases for those respective slopes.

Hopefully, this will give you a good "jumpstart". In any case, we can talk tomorrow (morning?), if you want. In case you do, please let me know.

Cheers,
António
